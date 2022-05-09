---
title:  "Sử dụng các mô hình deep learning trong xử lý ảnh với module DNN OpenCV"
header:
    # image: /images/2017-10-23-review-the-secret-life-of-walter-mitty/header_image.jpg
    caption: Deep Learning with OpenCV DNN Module
categories: 
    - Technology
tags:
    - machine-learning
    - computer-vision
    - opencv
---

## Load model với opencv


Xử lý ảnh là đề tài quen thuộc với mọi người. Hiện tại có rất nhiều thư viện hỗ trợ xử lý ảnh hoặc các thư viện deep learning như opencv, tensorflow, pytorch, ...

Tuy nhiên, để đáp ứng được tốc độ xử lý realtime thì nếu sử dụng ngôn ngữ lập trình bậc cao như Python là rất khó. Vì vậy xu hướng trong xử lý ảnh là sử dụng ngôn ngữ lập trình bậc trung như C++ hoặc nhúng trên client với js.

Nhưng thực tế là nếu phát triển model từ giai đoạn xử lý data, build model đến training và evaluation để đưa ra model factory mà sử dụng hoàn toàn là C++ là vô cùng khó khăn. Chính vì vậy chúng ta cần phát triển và triển khai model theo phương pháp sau:
- Phát triển model factory bằng ngôn ngữ bậc cao như Python với các thư viện như Pytorch, Tensorflow
- Serving model bằng C++


## Hướng xử lý cho phương pháp nêu ra

Hiện tại có nhiều cách serving model với C++ như sử tensorRT. Tuy nhiên cá nhân mình chưa tìm hiểu sâu về tensorRT nên không thể viết về nó được. Phương pháp hiện tại của mình chính là viết một thư viện C++ triển khai các layer, activation function bằng C++ sau đó load factory model để convert sang C++.
Điều khó khăn là viết lại thư viện deeplearning đòi hỏi rất nhiều kiến thức về ngôn ngữ lập trình C++ chuyên sâu kết hợp với kiến thức vững chắc về deeplearning. Thật may là thư viện opencv đã triển khai code wrapper các layer sang C++ (nhưng hiện tại chỉ có thể xử lý các mô hình thiên cho xử lý ảnh với nhân CNN)

## Cài đặt thư viện opencv

Trước hết mình xin hướng dẫn mọi người cài đặt opencv C++ trên Ubuntu (Ubuntu đã hỗ trợ gcc++)

#### Update Ubuntu system package

```console
sudo apt-get update && sudo apt-get upgrade
```

#### Install required tools and packages

```console
sudo apt-get install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```

#### Download OpenCV sources using git

```console
sudo -s
```

```console
cd /opt
```

```console
git clone https://github.com/Itseez/opencv.git

```

```console
git clone https://github.com/Itseez/opencv_contrib.git
```

#### Build and install opencv

```console
cd opencv
``` 

```console
mkdir release
```

```console
cd release
```

```console
cmake -D BUILD_TIFF=ON -D WITH_CUDA=OFF -D ENABLE_AVX=OFF -D WITH_OPENGL=OFF -D WITH_OPENCL=OFF -D WITH_IPP=OFF -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_EIGEN=OFF -D WITH_V4L=OFF -D WITH_VTK=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D OPENCV_GENERATE_PKGCONFIG=ON -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=/opt/opencv_contrib/modules /opt/opencv/
```

#### j4 - Num thread in your device
Ví dụ máy tính của bạn là 4 core 8 luồng thì chọn j8 nếu là 8 core 16 luồng thì chọn j16. Hoặc bạn hoàn toàn có thể chọn số luồng nhỏ hơn số luồng của computer. Như ví dụ mình sẽ chọn là 4 luồng (j4)
```console
make -j4
```

```console
make install
```

```console
ldconfig
```

```console
exit
cd ~
```

#### Find and set "opencv.pc" file path

```console
ls /usr/local/lib/pkgconfig/
```

```console
sudo cp /usr/local/lib/pkgconfig/opencv4.pc  /usr/lib/x86_64-linux-gnu/pkgconfig/opencv.pc
```
Câu lệnh trên mọi người sẽ hoàn toàn không tìm được ở bất cứ tutorial hướng dẫn cài opencv nào. Nên mình xin nhất mạnh là câu lệnh trên rất quan trọng, bất buộc phải có. Nếu không thì lúc build file với opencv ubuntu sẽ không phát hiện được opencv.

```console
pkg-config --modversion opencv
```

#### Build file with opencv

```console
g++ m.cpp -o app `pkg-config --cflags --libs opencv`
```
Trong đó m.cpp là filde C++ serving model của mình.


## Load model với C++

Bước tiếp theo là serving với C++. Lúc này chúng ta cần lưu weight cho model factory để triển khai. Nếu model bạn sử dụng keras và lưu dưới dạng file hdf5 thì cần load lại model với GraphDef sau đó freeze model dưới dạng file với đuôi mở rộng là .pb và .pbtxt. Dưới đây là code minh họa:

```python
import cv2
from keras.models import load_model
import numpy as np

from utils.datasets import get_labels
from utils.inference import detect_faces
from utils.inference import draw_text
from utils.inference import draw_bounding_box
from utils.inference import apply_offsets
from utils.inference import load_detection_model
from utils.preprocessor import preprocess_input

# parameters for loading data and images
import tensorflow as tf
from tensorflow import keras
from tensorflow.python.framework.convert_to_constants import convert_variables_to_constants_v2
import numpy as np
#path of the directory where you want to save your model
frozen_out_path = ''
# name of the .pb file
frozen_graph_filename = "frozen_graph"
model = load_model("../trained_models/emotion_models/fer2013_mini_XCEPTION.102-0.66.hdf5")
# Convert Keras model to ConcreteFunction
full_model = tf.function(lambda x: model(x))
full_model = full_model.get_concrete_function(
    tf.TensorSpec(model.inputs[0].shape, model.inputs[0].dtype))
# Get frozen ConcreteFunction
frozen_func = convert_variables_to_constants_v2(full_model)
frozen_func.graph.as_graph_def()
layers = [op.name for op in frozen_func.graph.get_operations()]
print("-" * 60)
print("Frozen model layers: ")
for layer in layers:
    print(layer)
print("-" * 60)
print("Frozen model inputs: ")
print(frozen_func.inputs)
print("Frozen model outputs: ")
print(frozen_func.outputs)
# Save frozen graph to disk
tf.io.write_graph(graph_or_graph_def=frozen_func.graph,
                  logdir=frozen_out_path,
                  name=f"{frozen_graph_filename}.pb",
                  as_text=False)
# Save its text representation
tf.io.write_graph(graph_or_graph_def=frozen_func.graph,
                  logdir=frozen_out_path,
                  name=f"{frozen_graph_filename}.pbtxt",
                  as_text=True)

```

Sau khi có model factory dưới dạng file .pb và .pbtxt thì ở C++ chúng ta sẽ triển khai như sau:

```cpp
using namespace dnn;

class Singleton {

    private:
        static Singleton *instance;
        Net net;
        bool isLoadModel = false;

    public:
         // This is how clients can access the single instance
        static Singleton* getInstance(){
            if(!instance){
                instance = new Singleton();
            }

            return instance;
        }

        Net getModel(){
            // load model one time 
            if(!this->isLoadModel){
                cout << "Load model " << endl;
                net = cv::dnn::readNetFromTensorflow("trained_models/frozen_graph.pb");
                this->isLoadModel = true;
            }

            return net;
        }
};

// Define the static Singleton pointer
Singleton* Singleton::instance = 0;

// int main() {

//    Singleton* p1 = Singleton::getInstance();
    
//    Singleton* p2 = Singleton::getInstance();
//    Net net1 = p1 -> getModel();
//    Net net2 = p2 -> getModel();
// }

```
Hàm readNetFromTensorflow ưu cầu cả file .pb và file .pbtxt nhưng thực tế mình sử dụng thì khi freeze model nếu chúng ta sử dụng file .pbtxt thì sẽ lỗi, mình cũng không rõ nguyên nhân tại sao. Thế nên ở bước load model này mình chỉ cần sử dụng file .pb là đủ. 
Mọi người có nhận thấy là load model mình sử dụng singleton design pattern.

```cpp
#include <iostream>
#include <vector>
#include <fstream>
#include <sstream>
#include <chrono>
#include <string>

#include <opencv2/dnn.hpp>
#include <opencv2/highgui.hpp>
#include <opencv2/opencv.hpp>
#include <opencv2/objdetect/objdetect.hpp>
#include <opencv2/imgproc.hpp>
#include "singleton.cpp"

using namespace cv;
using namespace std;
using namespace dnn;
using namespace std::chrono;



void prediction(Mat& frame)
{
    CascadeClassifier face_detector;
    face_detector.load("./trained_models/haarcascade_frontalface_default.xml");
    Singleton *p = Singleton::getInstance();

    Net emotion_classifier = p->getModel();

    // cv::Mat frame;

    vector<Rect> faces;

    // // Load image with gray and rgb channel
    cv::Mat img_gray;
    cv::cvtColor(frame, img_gray, cv::COLOR_BGR2GRAY);
    // imread opencv read image with BGR
    // Mat img_gray;
    // cv::cvtColor(frame, img_gray, cv::COLOR_BGR2GRAY);

    // Detect face in image gray
    face_detector.detectMultiScale(img_gray, faces, 1.3, 5);
    cv::imshow("time", img_gray);

    for (int i = 0; i < faces.size(); i++)
    {
        Mat face = img_gray(faces[i]);
        resize(face, face, Size(64, 64));

        // Create 4-Dimension input
        cv::Mat blob = cv::dnn::blobFromImage(face, 1 / 255.F, cv::Size(64, 64), cv::Scalar(), false, false);

        emotion_classifier.setInput(blob);
        // Predict
        Mat result = emotion_classifier.forward().clone();
        float m = 0.0;
        int index;
        for (int j = 0; j <= 6; j++){
            if (result.at<float>(0, 0, j) > m){
                m = result.at<float>(0, 0, j);
                index = j;
            }
        }
        String text;
        if (index == 0){
            text = "angry";
        }
        else if (index == 1){
            text = "disgust";
        }
        else if (index == 2){
            text = "fear";
        }
        else if (index == 3){
            text = "happy";
        }
        else if (index == 4){
            text = "sad";
        } else if (index == 5){
            text = "surprise";
        }
        else {
            text = "neutral";
        }

        // draw bounding box and text
        cv::rectangle(frame, faces[i].tl(), faces[i].br(), Scalar(255, 0, 255), 2);
        cv::putText(frame, text, cv::Point(faces[i].tl()), cv::FONT_HERSHEY_COMPLEX_SMALL, 2.0, cv::Scalar(0, 0, 255), 2.0, cv::LINE_AA);
    }
}

```

Trong opencv C++ thì image sẽ được lưu dưới dạng struct là Mat. Do bài toán của mình là sử dụng ảnh xám nên code tạo blobFromImage sẽ là như sau:
```cpp
cv::Mat blob = cv::dnn::blobFromImage(face, 1 / 255.F, cv::Size(64, 64), cv::Scalar(), false, false);

```

Sẽ có 1 dimension toàn là 0, nếu mọi người sử dụng ảnh với 3 channel RGB thì cần thay đổi một chút ở câu lệnh này.

## Lời kết
Như vậy, mình vừa viết hướng dẫn cơ bản để sử dụng opencv cho load model deeplearning được phát triển với Python. Hiện tại, để đáp ứng tốc độ xử lý cũng như nhúng vào các edge device thì cần serving model với C++ là tất yếu.
Hi vọng qua bài viết này, mọi người hoàn toàn có thể làm lại và chính xác. Mọi thắc mắc hãy comment ở đây hoặc ping mình trên Slack, mình sẽ phản hồi sớm nhất có thể.
Thanks for reading !!!!