---
title: Django - Models
toc: true
categories:
    - Technology
tags:
    - framework
---

This post will focus on building models in Django framework


## Best Practice: Naming
Model name should be singular noun.

Related name should be plural noun.
```py
class Owner(models.Model):
    pass
class Item(models.Model):
    owner = models.ForeignKey(Owner, related_name='items')
```

Use `verbose_name` to specify the name of the field.

### Order of Things in Models
constants (for choices and other)
- fields of the model
- custom manager/default manager
- meta class
- def __str__(self):
- other special methods[class or static or methods]
- def clean
- def save
- def get_absolute_url


## Best Practice: Business Logic
The best place to allocate business logic for your project is in models, namely method models and model manager. 


Use `ModelName.DoesNotExist` instead of `ObjectDoesNotExist`.



## Model Fields

### Meta class
Add some metadata about the model. For example: verbose_name, verbose_name_plural



### Dirty data should not be found in a base
Use `PositiveIntegerField` instead of `IntegerField`. For the same reason, we should use `unique`, `unique_together`, `required`.

### Using `help_text` as documentation


### Money Information Storage
Use `DecimalField` instead of `FloatField` for money field.


### Don't use `null=True` if you don't need it


### Multiple choices
Choices is a list of tuples. Inside each choice, the first is the value and the second is the display value.

Choices should be placed inside the model so it can be used as a static variable.

Example:
```py
WEEKDAY_CHOICES = (
    (0, 'everyday'),
    (1, 'monday'),
    (2, 'tuesday'),
    (3, 'wednesday'),
    (4, 'thursday'),
    (5, 'friday'),
    (6, 'saturday'),
    (7, 'sunday'),
)

class MyClass(models.Model):
    ...
    notification_weekday = models.CharField(choices=WEEKDAY_CHOICES, default=0)
    ...
```

We can do it even better with enum approach.
```py
EVERYDAY = 0
MONDAY = 1

WEEKDAY_CHOICES = (
    (EVERYDAY, 'everyday'),
    (MONDAY, 'monday'),
    ...
)
```

Choice field is an interesting topic so that many developers have working on creating library [`model_utils`](https://django-model-utils.readthedocs.io/en/stable/) that can reduce the overhead of creating choice field (for example, statuses field).

```py
from model_utils import Choices

class Article(models.Model):
    STATUSES = Choices(
        (0, 'draft', _('draft')),
        (1, 'published', _('published')))
    status = models.IntegerField(choices=STATUSES, default=STATUSES.draft)
    ...
```



### Use abstract models
Use abstract to share some logic between models

```py
class Meta:
    abstract = True
```

### Use custom Manager or QuerySet

Example: Manager
```py
class CustomManager(models.Manager):
        def with_comments_counter(self):
            return self.get_queryset().annotate(comments_count=Count('comment_set'))

# Now you can use:
posts = Post.objects.with_comments_counter()
posts[0].comments_count
```

Example: QuerySet
```py
class CustomQuerySet(models.query.QuerySet):
    """Substitution the QuerySet, and adding additional methods to QuerySet
    """

    def with_comments_counter(self):
        """Adds comments counter to queryset"""
        return self.annotate(comments_count=Count('comment_set'))

posts = Post.objects.filter(...).with_comments_counter()
posts[0].comments_count
```



## References:
- https://steelkiwi.com/blog/best-practices-working-django-models-python/
- https://medium.com/@balaraju.mme/best-practices-for-django-models-in-python-b47ff6e78966
