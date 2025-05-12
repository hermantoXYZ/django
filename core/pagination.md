---
icon: ellipsis-stroke-vertical
---

# Pagination



<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Tambahkan statment dalam views.py atau logika app..

```python
from django.core.paginator import Paginator
```

Berikut fungsi singkatnya.. silahkan disimak..

```python
def news_lists(request):
    news_lists = PostNews.objects.all().order_by('-created_at')
    
    paginator = Paginator(news_lists, 6)  
    page_number = request.GET.get('page')  L
    page_obj = paginator.get_page(page_number)  

    return render(request, 'myapp/article_list.html', {'page_obj': page_obj})
```

Pastikan dari `views.py`tersebut telah tersetUp dengan baik, mulai dari urls.py dan `HTML`nya ya..

Ini saya tampilkan templates HTML dari projects saya, silahkan digunakan

```html
                <div class="row justify-content-center mt-4">
                  <div class="col-auto">
                      <nav aria-label="Page navigation">
                          <ul class="pagination">
                              <!-- Tombol Previous -->
                              <div data-gb-custom-block data-tag="if">
                              <li class="page-item">
                                  <a class="page-link" href="?page=1" aria-label="First">
                                      <span aria-hidden="true">&laquo;&laquo;</span>
                                  </a>
                              </li>
                              <li class="page-item">
                                  <a class="page-link" href="?page={{ page_obj.previous_page_number }}" aria-label="Previous">
                                      <span aria-hidden="true">&laquo;</span>
                                  </a>
                              </li>
                              <div data-gb-custom-block data-tag="else"></div>
                              <li class="page-item disabled">
                                  <span class="page-link">&laquo;</span>
                              </li>
                              </div>
              
                              <!-- Nomor Halaman -->
                              <div data-gb-custom-block data-tag="for">
                                  <div data-gb-custom-block data-tag="if">
                                  <li class="page-item active"><span class="page-link">{{ num }}</span></li>
                                  <div data-gb-custom-block data-tag="elif" data-0='-3' data-1='-3' data-2='-3' data-3='-3' data-4='-3' data-5='-3' data-6='-3' data-7='3' data-8=' and num < page_obj.number|add:' data-9='3'></div>
                                  <li class="page-item"><a class="page-link" href="?page={{ num }}">{{ num }}</a></li>
                                  </div>
                              </div>
              
                              <!-- Tombol Next -->
                              <div data-gb-custom-block data-tag="if">
                              <li class="page-item">
                                  <a class="page-link" href="?page={{ page_obj.next_page_number }}" aria-label="Next">
                                      <span aria-hidden="true">&raquo;</span>
                                  </a>
                              </li>
                              <li class="page-item">
                                  <a class="page-link" href="?page={{ page_obj.paginator.num_pages }}" aria-label="Last">
                                      <span aria-hidden="true">&raquo;&raquo;</span>
                                  </a>
                              </li>
                              <div data-gb-custom-block data-tag="else"></div>
                              <li class="page-item disabled">
                                  <span class="page-link">&raquo;</span>
                              </li>
                              </div>
                          </ul>
                      </nav>
                  </div>
              </div>
```

Saya lengkapi dengan cssnya,&#x20;

```css

/* Page */

.pagination {
    display: flex;
    padding-left: 0;
    list-style: none
}

.page-link {
    position: relative;
    display: block;
    color: #000000;
    background-color: #fff;
    border: 1px solid #dee2e6;
    transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out
}

@media(prefers-reduced-motion: reduce) {
    .page-link {
        transition: none
    }
}

.page-link:hover {
    z-index: 2;
    color: #014ca1;
    background-color: #e9ecef;
    border-color: #dee2e6
}

.page-link:focus {
    z-index: 3;
    color: #014ca1;
    background-color: #e9ecef;
    outline: 0;
    box-shadow: 0 0 0 .25rem rgba(1,95,201,.25)
}

.page-item:not(:first-child) .page-link {
    margin-left: -1px
}

.page-item.active .page-link {
    z-index: 3;
    color: #fff;
    background-color: #ffc107;
    border-color: #ffc107
}

.page-item.disabled .page-link {
    color: #6c757d;
    pointer-events: none;
    background-color: #fff;
    border-color: #dee2e6
}

.page-link {
    padding: .375rem .75rem
}

.page-item:first-child .page-link {
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px
}

.page-item:last-child .page-link {
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px
}

.pagination-lg .page-link {
    padding: .75rem 1.5rem;
    font-size: 1.25rem
}

.pagination-lg .page-item:first-child .page-link {
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px
}

.pagination-lg .page-item:last-child .page-link {
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px
}

.pagination-sm .page-link {
    padding: .25rem .5rem;
    font-size: 0.875rem
}

.pagination-sm .page-item:first-child .page-link {
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px
}

.pagination-sm .page-item:last-child .page-link {
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px
}

/* Page */
```

Cukup itu, page dari halaman listt itu berhasil dibuat ya..

{% code title="Ini optional ya, tapi kalian harus perhatikan isi dari list objectnya views kalian.." %}
```html
<div class="categories-list-undangan">
  <div data-gb-custom-block data-tag="for">
    <div class="category-list-undangan">
      <img src="<div data-gb-custom-block data-tag="if">{{ post.images.url }}<div data-gb-custom-block data-tag="else"></div><div data-gb-custom-block data-tag="static" data-0='img/no-image.png'></div></div>" style="height: 80px; width: 100px;" alt="{{ post.title }}">
      <div class="category-text">
        <p><a href="<div data-gb-custom-block data-tag="url" data-0='edit_news'></div>">{{ post.title|truncatechars:90 }}</a></p>
        <p>By: {{ post.author.username }}</p>
        <p>Category: {{ post.category.name }}</p>
        <p>Published: {{ post.created_at|date:"d M Y H:i" }}</p>
      </div>
      <div class="menu-list-undangan">
        <i class="fas fa-ellipsis-v menu-icon" onclick="toggleModal('modal{{ post.id }}')"></i>
        <div id="modal{{ post.id }}" class="modal">
          <ul>
            <a href="<div data-gb-custom-block data-tag="url" data-0='news_detail'></div>">
              <li><i class="fas fa-eye"></i> View Post</li>
            </a>
            <a href="<div data-gb-custom-block data-tag="url" data-0='edit_news'></div>">
              <li><i class="fas fa-edit"></i> Edit Post</li>
            </a>
            <form id="delete-form-{{ post.id }}" method="POST" action="<div data-gb-custom-block data-tag="url" data-0='delete_news'></div>" style="display:inline;">
              <div data-gb-custom-block data-tag="csrf_token"></div>
              <input type="hidden" name="delete_post_id" value="{{ post.id }}">
              <a href="#" class="btn-style" 
                 onclick="event.preventDefault(); if(confirm('Are you sure you want to delete this post?')) { document.getElementById('delete-form-{{ post.id }}').submit(); }">
                  <li><i class="fas fa-trash"></i> Delete Post</li>
              </a>
          </form>
          </ul>
        </div>
      </div>
    </div>
  <div data-gb-custom-block data-tag="empty"></div>
    <p class="text-center">No news available.</p>
  </div>
</div>

```
{% endcode %}
