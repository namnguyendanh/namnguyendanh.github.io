---
title:  "Nature of Code: Random Walks"
toc: true
categories:
    - Technology
tags:
    - simulation
---

## Introduction
A **traditional random walker** can move in 4 directions: up, down, left, right. Each direction has a probability of 25%.
### Creating Walker Class in Processing
```c
class Walker {
  // position
  int x;
  int y;

  // Constructor
  Walker() {
    x = width/2;
    y = height/2;
  }

  void display() {
    stroke(0);
    point(x,y);
  }
```
To define the way the random walker move, we need to define `step()` method.
```c
void step(){
  int choice = int(random(3)) // 0, 1, 2, 3

  if(choice == 0){
    x++;
  }
  else if(choice == 1){
    x--;
  }
  else if(choice == 2){
    y++;
  }
  else{
    y--;
  }

}

```
We can improve this by adding the direction vector `v = (stepx, stepy)`, which means `new_position = old_position + v`
```c
void step(){
  scale = 1
  float stepx = random(-1, 1);
  float stepy = random(-1, 1);

  x += stepx * scale;
  y += stepy * scale;
}
```
In processing, `random()` produces what is known as a **Uniform distribution** of numbers. 

We can create a random walker that tends to move to the left. Here is an example of a Walker with the following probabilities:
- chance of moving up: 20%
- chance of moving down: 20%
- chance of moving left: 40%
- chance of moving right: 20%

```c
void step(){
  float choice = random(1); //[0, 1)

  if(choice < 0.4){ // a 40% chance of moving to the left
    x--;
  }
  else if(r < 0.6){
    x++;
  }
  else if(r < 0.8){
    y++;
  }
  else{
    y--;
  }
}
```

## Normal distribution
**Gaussian distribution (normal distribution)**: a distribution of values that cluster around an average(or mean).
```c
void setup(){
  size(640,360);
  generator = new Random();
}
```

```c
void draw(){
  float num = (float)generator.nextGaussian(); // mean = 0, standand deviation = 1 => [-1, 1]

  float sd = 60;
  float mean = 320;

  float r = sd * num + mean;
}
```

## Perlin Noise
Perlin noise can be used to generate various effects with natural qualities, such as clouds, landscapes, and patterned textures like marble.

![Perlin Noise][perlin_noise]

```c
float t = 0;

void draw() {
  float n = noise(t); // range between [0, 1]

  float x = map(n,0,1,0,width); //mapping from [0,1] to [0,width]

  t += 0.01; //Change this will affect the smoothness of the noise
}
```
We can apply Perlin noise in Random walker
```c
float t = 0;
void step(){
  x = map(noise(t),0,1,0,640);
  y = map(noise(t+30),0,1,0,360);

  t += 0.01;
}

```
![Random Walker with Perlin noise][perlin_noise_random_walker]

### 2D Perlin noise
```c
void setup(){
   size(640, 360);
}

void draw(){
  float xoff = 0.0;
  for(int x=0; x<640; x++){
    float yoff = 0.0;
     for(int y=0; y<360; y++){
        float bright = map(noise(xoff, yoff), 0, 1, 0, 255);
        stroke(bright);
        point(x, y);
        yoff += 0.01;
     }
    xoff += 0.01;
  }
  
  save("2D-noise.png");
}
```
![2D Perlin noise][2D-noise.png]

[perlin_noise]: http://natureofcode.com/book/imgs/intro/intro_05.png
[perlin_noise_random_walker]: /images/2018-06-04-nature-of-code-random-walks/perlin-noise-random-walker.png
[2D-noise.png]: /images/2018-06-04-nature-of-code-random-walks/2D-noise.png