---
icon: books-medical
---

# Create Placeholders

To add placeholders for the fields in your `AddNewsForm`, you can use the `widgets` attribute inside the `Meta` class. Each field can have its own widget with a `attrs` dictionary, where you define the placeholder text.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption><p>Before..</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>After</p></figcaption></figure>

Here's how your updated code would look with placeholders:

```python
class AddNewsForm(TranslatableModelForm):
    # content = forms.CharField(widget=CKEditorWidget())
    class Meta:
        model = PostNews
        fields = ['title', 'content', 'images', 'category', 'slug']

        widgets = {
            'title': forms.TextInput(attrs={'placeholder': 'Enter the news title'}),
            'images': forms.ClearableFileInput(attrs={'placeholder': 'Upload related images'}),
            'category': forms.Select(attrs={'placeholder': 'Choose a category'}),
            'slug': forms.TextInput(attrs={'placeholder': 'Enter a unique slug (optional)'}),
        }
```

