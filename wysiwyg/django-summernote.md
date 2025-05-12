---
icon: typewriter
---

# Django-summernote

Install `django-summernote` to your python environment.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

Kemudian.. add \``django_summernote` to app setting.py

```
// Some code
INSTALLED_APPS += ('django_summernote', )
```

```python
// Some code
from django.urls import include
# ...
urlpatterns = [
    ...
    path('summernote/', include('django_summernote.urls')),
    ...
]
```

Tambahkan ke url Project bukan URL APP..

1. Be sure to set proper `MEDIA_URL` for attachments.
   *   The following is an example test code:

       ```
       MEDIA_URL = '/media/'
       MEDIA_ROOT = os.path.join(BASE_DIR, 'media/')
       ```
   *   When debug option is enabled(`DEBUG=True`), DO NOT forget to add urlpatterns as shown below:

       ```
       from django.conf import settings
       from django.conf.urls.static import static

       if settings.DEBUG:
           urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
       ```
   * Please, read the [official v3.0 documentation](https://docs.djangoproject.com/en/3.0/topics/files/) for more details on file uploads.
2.  Run database migration for preparing attachment model.

    <pre><code><strong>python manage.py makemigrations
    </strong>python manage.py migrate
    </code></pre>
3.  Collect static files before publishing or development.

    ```
    python manage.py collectstatic
    ```

### Setelah itu..

Apply summernote to `forms.py`

```python
from django_summernote.widgets import SummernoteWidget, SummernoteInplaceWidget
```

Full code display berada di bawah.

{% code title="" overflow="wrap" lineNumbers="true" %}
```python
content = forms.CharField(widget=SummernoteWidget())  # instead of forms.Textarea
    class Meta:
        model = PostNews
        fields = ['content']
```
{% endcode %}

````python
// Some code
class AddNewsForm(TranslatableModelForm):
    content = forms.CharField(widget=SummernoteWidget())  # instead of forms.Textarea
    class Meta:
        model = PostNews
        fields = ['title', 'author', 'content', 'images', 'category', 'slug', 'created_at']

        widgets = {
            'title': forms.TextInput(attrs={'placeholder': 'Enter the news title'}),
            'images': forms.ClearableFileInput(attrs={'placeholder': 'Upload related images'}),
            'author': forms.Select(attrs={
                'class': 'form-control',  # Menambahkan kelas CSS (opsional)
            }),
            'category': forms.Select(attrs={
                'class': 'form-control',  # Menambahkan kelas CSS (opsional)
            }),
            'slug': forms.TextInput(attrs={'placeholder': 'Enter a unique slug (optional)'}),
            'created_at': forms.DateTimeInput(
                attrs={
                    'type': 'datetime-local',  # Use HTML5 datetime-local input
                    'placeholder': 'Select date and time',

                }
            )
        }

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['category'].empty_label = "Pilih Kategori"
        self.fields['author'].empty_label = "Pilih Penulis"

        
```
````

Agar antara Admin.py dan form html yang dibuat dapat berfungsi dengan baik, antara keduanya, silahkan update `admin.py`

```python
class PostNewsAdminForm(forms.ModelForm):
    # content = forms.CharField(widget=CKEditorWidget())  # Gunakan CKEditor untuk content
    content = forms.CharField(widget=SummernoteWidget())
    class Meta:
        model = PostNews
        fields = '__all__'

    def __init__(self, *args, **kwargs):
        super(PostNewsAdminForm, self).__init__(*args, **kwargs)
        if self.instance and self.instance.pk:
            self.fields['content'].initial = self.instance.content

```
