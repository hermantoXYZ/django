---
icon: python
---

# Panel Admin

I will sharing costomize panel adming using \`jazzmin\`

adding `jazzmin`

dalam install app seeting.py



<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Error, karena database belum di migrate..

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

`python manage.py migrate`

## Create Super User

```
// Some code
python manage.py createsuperuser
```

Lanjutkan untuk isi

```
// Some code
(venv) PS D:\REACT NATIVE\DJANGO\blogreact\backend> python manage.py createsuperuser
Username (leave blank to use 'legion'): admin
Email address: andi@gmail.com
Password:
Password (again):
Superuser created successfully.
(venv) PS D:\REACT NATIVE\DJANGO\blogreact\backend> python manage.py runserver      
```

Dan silahkan login dengan akun super user..

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>



Disini saya akan berikan costume dari jazzmin project saya.

```
// Some code

# Custom Admin Settings
JAZZMIN_SETTINGS = {
    "site_title": "Desphixs",
    "site_header": "Desphixs",
    "site_brand": "Modern Marketplace ",
    # "site_icon": "images/favicon.ico",
    # "site_logo": "images/logos/logo.jpg",
    "welcome_sign": "Welcome To Desphixs",
    "copyright": "Desphixs",
    "user_avatar": "images/photos/logo.jpg",
    "topmenu_links": [
        {"name": "Dashboard", "url": "home", "permissions": ["auth.view_user"]},
        {"model": "auth.User"},
    ],
    "show_sidebar": True,
    "navigation_expanded": True,
    "order_with_respect_to": [
        "api",
        "api.Post",
        "api.Category",
        "api.Comment",
        "api.Bookmark",
        "api.Notification",
    ],
    "icons": {
        "admin.LogEntry": "fas fa-file",

        "auth": "fas fa-users-cog",
        "auth.user": "fas fa-user",

        "api.User": "fas fa-user",
        "api.Profile":"fas fa-address-card",
        "api.Post":"fas fa-th",
        "api.Category":"fas fa-tag",
        "api.Comment":"fas fa-envelope",
        "api.Notification":"fas fa-bell",
        "api.Bookmark":"fas fa-heart",

        
    },
    "default_icon_parents": "fas fa-chevron-circle-right",
    "default_icon_children": "fas fa-arrow-circle-right",
    "related_modal_active": False,
    
    "custom_js": None,
    "show_ui_builder": True,
    
    "changeform_format": "horizontal_tabs",
    "changeform_format_overrides": {
        "auth.user": "collapsible",
        "auth.group": "vertical_tabs",
    },
}

# Jazzmin Tweaks

JAZZMIN_UI_TWEAKS = {
    "navbar_small_text": False,
    "footer_small_text": False,
    "body_small_text": True,
    "brand_small_text": False,
    "brand_colour": "navbar-indigo",
    "accent": "accent-olive",
    "navbar": "navbar-indigo navbar-dark",
    "no_navbar_border": False,
    "navbar_fixed": False,
    "layout_boxed": False,
    "footer_fixed": False,
    "sidebar_fixed": False,
    "sidebar": "sidebar-dark-indigo",
    "sidebar_nav_small_text": False,
    "sidebar_disable_expand": False,
    "sidebar_nav_child_indent": False,
    "sidebar_nav_compact_style": False,
    "sidebar_nav_legacy_style": False,
    "sidebar_nav_flat_style": False,
    "theme": "default",
    "dark_mode_theme": None,
    "button_classes": {
        "primary": "btn-outline-primary",
        "secondary": "btn-outline-secondary",
        "info": "btn-info",
        "warning": "btn-warning",
        "danger": "btn-danger",
        "success": "btn-success"
    }
}
```

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
