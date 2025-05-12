---
icon: python
---

# Create User & Profile Model

Berikut ini model databases user dan profil..

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.db.models.signals import post_save
from django.utils.html import mark_safe
from django.utils.text import slugify

from shortuuid.django_fields import ShortUUIDField
import shortuuid

class User(AbstractUser):
    username = models.CharField(unique=True, max_length=100)
    email = models.EmailField(unique=True) 
    full_name = models.CharField(max_length=100, null=True, blank=True)
    otp = models.CharField(max_length=100, null=True, blank=True)
    
    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['username']

    def __str__(self):
        return self.email
    
    def save(self, *args, **kwargs):
        email_username, mobile = self.email.split("@")
        if self.full_name == "" or self.full_name == None:
            self.full_name = email_username
        if self.username == "" or self.username == None:
            self.username = email_username  
    
        super(User, self).save(*args, **kwargs)


class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    image = models.FileField(upload_to="image", default="default/default-user.jpg", null=True, blank=True)
    full_name = models.CharField(max_length=100, null=True, blank=True)
    bio = models.TextField(null=True, blank=True)
    about = models.TextField(null=True, blank=True)
    author = models.BooleanField(default=False)
    country = models.CharField(max_length=100, null=True, blank=True)
    facebook = models.CharField(max_length=100, null=True, blank=True)
    twitter = models.CharField(max_length=100, null=True, blank=True)
    date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        if self.full_name:
            return str(self.full_name)
        else:
            return str(self.user.full_name)
    

    def save(self, *args, **kwargs):
        if self.full_name == "" or self.full_name == None:
            self.full_name = self.user.full_name
        super(Profile, self).save(*args, **kwargs)

    def thumbnail(self):
        return mark_safe('<img src="/media/%s" width="50" height="50" object-fit:"cover" style="border-radius: 30px; object-fit: cover;" />' % (self.image))
    

def create_user_profile(sender, instance, created, **kwargs):
    if created:
        Profile.objects.create(user=instance)

def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()

post_save.connect(create_user_profile, sender=User)
post_save.connect(save_user_profile, sender=User)

```



Saat migrasi database..comment&#x20;

```python
    'jazzmin',
    'django.contrib.admin',
```

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

dan...

```python
  path('admin/', admin.site.urls),
```

Lakukan hingga berhasil..

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

* py manage.py makemigrations api
* python manage.py migrate api

dan

* py manage.py makemigrations
* py manage.py migrate

<figure><img src="../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

Jika masih gagal, hapus folder `migrations` atau file dbsqlite3, baru lakukan ulang.  setelah berhasil lakukan adding admin.py

{% code title="" lineNumbers="true" %}
```python
from django.contrib import admin

# Register your models here.
from api import models as api_models

admin.site.register(api_models.User)
admin.site.register(api_models.Profile)
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>
