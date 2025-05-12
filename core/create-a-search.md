---
icon: magnifying-glass
---

# Create A Search

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### Saya menggunakan parameter GET dari request untuk mencari kata kunci pencarian dan melakukan filter berdasarkan hasil dari paramater.

Berhubung saya sudah punya views/logika untuk menampilkan list sebuah data, saya cukup menambahkan tambahkan logika pencarian.

{% code title="views.PY Before " overflow="wrap" lineNumbers="true" %}
```python
@user_passes_test(is_staff)
def news_lists(request):
    news_lists = PostNews.objects.all().order_by('-created_at')
    return render(request, 'dashboard/news_lists.html', {'news_lists': news_lists})
```
{% endcode %}

Menambahkan Statment

`from django.db.models import Q` dalam views.py

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

```
// Some code
 query = request.GET.get('q', '')  # Tangkap kata kunci pencarian dari parameter 'q'
```

Full code:

{% code title="" lineNumbers="true" %}
```python
@user_passes_test(is_staff)
def news_lists(request):
    query = request.GET.get('q', '')  # Tangkap kata kunci pencarian dari parameter 'q'


    news_lists = PostNews.objects.all().order_by('-created_at')

    if query:
        news_lists = news_lists.filter(
            Q(translations__title__icontains=query) |  # Filter berdasarkan title
            Q(translations__content__icontains=query)  # Filter berdasarkan content
        ).distinct()  # Pastikan tidak ada duplikasi

    return render(request, 'dashboard/news_lists.html', {'news_lists': news_lists, 'query': query})
```
{% endcode %}

## HTML

{% code lineNumbers="true" %}
```html
 <form method="GET" action="/" class="search-form">
        <input type="text" name="q" placeholder="Search News..." value="{{ request.GET.q }}">
        <button type="submit"><i class="fas fa-search"></i></button>
      </form>
```
{% endcode %}

{% code lineNumbers="true" %}
```html
      <div data-gb-custom-block data-tag="for">
                <div data-gb-custom-block data-tag="empty"></div>
                <form method="get" action="" class="search-form">
                  <input type="text" name="q" placeholder="Search news..." value="{{ query }}">
                  <button type="submit">Search</button>
              </form>
      </div>
```
{% endcode %}
