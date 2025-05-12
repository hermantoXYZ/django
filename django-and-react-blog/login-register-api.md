---
icon: python
---

# Login, Register API

Disini saya akan menyajikan cara membuat rest framework untuk react blog..

```python
  # Third-party apps
    'rest_framework',
    'rest_framework_simplejwt.token_blacklist',
    'drf_yasg',
    'corsheaders',
```

Pastikan telah diadding di setting.py APP INSTALL

Create file serializ.py

```python
from django.contrib.auth.password_validation import validate_password
from rest_framework_simplejwt.serializers import TokenObtainPairSerializer
from rest_framework import serializers
from rest_framework.validators import UniqueValidator
from rest_framework_simplejwt.serializers import TokenObtainPairSerializer

from api import models as api_models

# Define a custom serializer that inherits from TokenObtainPairSerializer
class MyTokenObtainPairSerializer(TokenObtainPairSerializer):
    '''
    class MyTokenObtainPairSerializer(TokenObtainPairSerializer):: This line creates a new token serializer called MyTokenObtainPairSerializer that is based on an existing one called TokenObtainPairSerializer. Think of it as customizing the way tokens work.
    @classmethod: This line indicates that the following function is a class method, which means it belongs to the class itself and not to an instance (object) of the class.
    def get_token(cls, user):: This is a function (or method) that gets called when we want to create a token for a user. The user is the person who's trying to access something on the website.
    token = super().get_token(user): Here, it's asking for a regular token from the original token serializer (the one it's based on). This regular token is like a key to enter the website.
    token['full_name'] = user.full_name, token['email'] = user.email, token['username'] = user.username: This code is customizing the token by adding extra information to it. For example, it's putting the user's full name, email, and username into the token. These are like special notes attached to the key.
    return token: Finally, the customized token is given back to the user. Now, when this token is used, it not only lets the user in but also carries their full name, email, and username as extra information, which the website can use as needed.
    '''
    @classmethod
    # Define a custom method to get the token for a user
    def get_token(cls, user):
        # Call the parent class's get_token method
        token = super().get_token(user)

        # Add custom claims to the token
        token['full_name'] = user.full_name
        token['email'] = user.email
        token['username'] = user.username
        try:
            token['vendor_id'] = user.vendor.id
        except:
            token['vendor_id'] = 0

        # ...

        # Return the token with custom claims
        return token

# Define a serializer for user registration, which inherits from serializers.ModelSerializer
class RegisterSerializer(serializers.ModelSerializer):
    # Define fields for the serializer, including password and password2
    password = serializers.CharField(write_only=True, required=True, validators=[validate_password])
    password2 = serializers.CharField(write_only=True, required=True)

    class Meta:
        # Specify the model that this serializer is associated with
        model = api_models.User
        # Define the fields from the model that should be included in the serializer
        fields = ('full_name', 'email',  'password', 'password2')

    def validate(self, attrs):
        # Define a validation method to check if the passwords match
        if attrs['password'] != attrs['password2']:
            # Raise a validation error if the passwords don't match
            raise serializers.ValidationError({"password": "Password fields didn't match."})

        # Return the validated attributes
        return attrs

    def create(self, validated_data):
        # Define a method to create a new user based on validated data
        user = api_models.User.objects.create(
            full_name=validated_data['full_name'],
            email=validated_data['email'],
        )
        email_username, mobile = user.email.split('@')
        user.username = email_username

        # Set the user's password based on the validated data
        user.set_password(validated_data['password'])
        user.save()

        # Return the created user
        return user

class UserSerializer(serializers.ModelSerializer):

    class Meta:
        model = api_models.User
        fields = '__all__'

class ProfileSerializer(serializers.ModelSerializer):

    class Meta:
        model = api_models.Profile
        fields = '__all__'

    def to_representation(self, instance):
        response = super().to_representation(instance)
        response['user'] = UserSerializer(instance.user).data
        return response

class PasswordResetSerializer(serializers.Serializer):
    email = serializers.EmailField()



class CategorySerializer(serializers.ModelSerializer):
    post_count = serializers.SerializerMethodField()

    '''
        category.post_set: In Django, when you define a ForeignKey relationship from one model to another 
        (e.g., Post model having a ForeignKey relationship to the Category model), 
        Django creates a reverse relationship from the related model back to the model that has the ForeignKey. 
        By default, this reverse relationship is named <model>_set. In this case, since the Post model has a 
        ForeignKey to the Category model, Django creates a reverse relationship from Category to Post named post_set. 
        This allows you to access all Post objects related to a Category instance.
    '''
    def get_post_count(self, category):
        return category.posts.count()
    
    class Meta:
        model = api_models.Category
        fields = [
            "id",
            "title",
            "image",
            "slug",
            "post_count",
        ]

    def __init__(self, *args, **kwargs):
        super(CategorySerializer, self).__init__(*args, **kwargs)
        request = self.context.get('request')
        if request and request.method == 'POST':
            self.Meta.depth = 0
        else:
            self.Meta.depth = 3

class CommentSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = api_models.Comment
        fields = "__all__"

    def __init__(self, *args, **kwargs):
        super(CommentSerializer, self).__init__(*args, **kwargs)
        request = self.context.get('request')
        if request and request.method == 'POST':
            self.Meta.depth = 0
        else:
            self.Meta.depth = 1


class PostSerializer(serializers.ModelSerializer):
    comments = CommentSerializer(many=True)
    
    class Meta:
        model = api_models.Post
        fields = "__all__"

    def __init__(self, *args, **kwargs):
        super(PostSerializer, self).__init__(*args, **kwargs)
        request = self.context.get('request')
        if request and request.method == 'POST':
            self.Meta.depth = 0
        else:
            self.Meta.depth = 3



class BookmarkSerializer(serializers.ModelSerializer):
    
    class Meta:
        model = api_models.Bookmark
        fields = "__all__"


    def __init__(self, *args, **kwargs):
        super(BookmarkSerializer, self).__init__(*args, **kwargs)
        request = self.context.get('request')
        if request and request.method == 'POST':
            self.Meta.depth = 0
        else:
            self.Meta.depth = 3
    
class NotificationSerializer(serializers.ModelSerializer):  

    class Meta:
        model = api_models.Notification
        fields = "__all__"

    def __init__(self, *args, **kwargs):
        super(NotificationSerializer, self).__init__(*args, **kwargs)
        request = self.context.get('request')
        if request and request.method == 'POST':
            self.Meta.depth = 0
        else:
            self.Meta.depth = 3

class AuthorStats(serializers.Serializer):
    views = serializers.IntegerField(default=0)
    posts = serializers.IntegerField(default=0)
    likes = serializers.IntegerField(default=0)
    bookmarks = serializers.IntegerField(default=0)
```

```python
from django.shortcuts import render
from django.http import JsonResponse
from django.core.mail import EmailMultiAlternatives
from django.template.loader import render_to_string
from django.conf import settings
from django.contrib.auth.tokens import default_token_generator
from django.utils.http import urlsafe_base64_encode
from django.utils.encoding import force_bytes
from django.db.models import Sum
# Restframework
from rest_framework import status
from rest_framework.decorators import api_view, APIView
from rest_framework.response import Response
from rest_framework_simplejwt.views import TokenObtainPairView
from rest_framework import generics
from rest_framework.permissions import AllowAny, IsAuthenticated
from rest_framework.decorators import api_view, permission_classes
from rest_framework_simplejwt.tokens import RefreshToken

from drf_yasg import openapi
from drf_yasg.utils import swagger_auto_schema
from datetime import datetime

# Others
import json
import random

# Custom Imports
from api import serializ as api_serializer
from api import models as api_models


# This code defines a DRF View class called MyTokenObtainPairView, which inherits from TokenObtainPairView.
class MyTokenObtainPairView(TokenObtainPairView):
    # Here, it specifies the serializer class to be used with this view.
    serializer_class = api_serializer.MyTokenObtainPairSerializer

# This code defines another DRF View class called RegisterView, which inherits from generics.CreateAPIView.
class RegisterView(generics.CreateAPIView):
    # It sets the queryset for this view to retrieve all User objects.
    queryset = api_models.User.objects.all()
    # It specifies that the view allows any user (no authentication required).
    permission_classes = (AllowAny,)
    # It sets the serializer class to be used with this view.
    serializer_class = api_serializer.RegisterSerializer


# This code defines another DRF View class called ProfileView, which inherits from generics.RetrieveAPIView and used to show user profile view.
class ProfileView(generics.RetrieveUpdateAPIView):
    permission_classes = (AllowAny,)
    serializer_class = api_serializer.ProfileSerializer

    def get_object(self):
        user_id = self.kwargs['user_id']

        user = api_models.User.objects.get(id=user_id)
        profile = api_models.Profile.objects.get(user=user)
        return profile
    

def generate_numeric_otp(length=7):
        # Generate a random 7-digit OTP
        otp = ''.join([str(random.randint(0, 9)) for _ in range(length)])
        return otp

class PasswordEmailVerify(generics.RetrieveAPIView):
    permission_classes = (AllowAny,)
    serializer_class = api_serializer.UserSerializer
    
    def get_object(self):
        email = self.kwargs['email']
        user = api_models.User.objects.get(email=email)
        
        if user:
            user.otp = generate_numeric_otp()
            uidb64 = user.pk
            
             # Generate a token and include it in the reset link sent via email
            refresh = RefreshToken.for_user(user)
            reset_token = str(refresh.access_token)

            # Store the reset_token in the user model for later verification
            user.reset_token = reset_token
            user.save()

            link = f"http://localhost:5173/create-new-password?otp={user.otp}&uidb64={uidb64}&reset_token={reset_token}"
            
            merge_data = {
                'link': link, 
                'username': user.username, 
            }
            subject = f"Password Reset Request"
            text_body = render_to_string("email/password_reset.txt", merge_data)
            html_body = render_to_string("email/password_reset.html", merge_data)
            
            msg = EmailMultiAlternatives(
                subject=subject, from_email=settings.FROM_EMAIL,
                to=[user.email], body=text_body
            )
            msg.attach_alternative(html_body, "text/html")
            msg.send()
        return user
    

class PasswordChangeView(generics.CreateAPIView):
    permission_classes = (AllowAny,)
    serializer_class = api_serializer.UserSerializer
    
    def create(self, request, *args, **kwargs):
        payload = request.data
        
        otp = payload['otp']
        uidb64 = payload['uidb64']
        password = payload['password']

        

        user = api_models.User.objects.get(id=uidb64, otp=otp)
        if user:
            user.set_password(password)
            user.otp = ""
            user.save()
            
            return Response( {"message": "Password Changed Successfully"}, status=status.HTTP_201_CREATED)
        else:
            return Response( {"message": "An Error Occured"}, status=status.HTTP_500_INTERNAL_SERVER_ERROR)

######################## Post APIs ########################
        

class CategoryListAPIView(generics.ListAPIView):
    serializer_class = api_serializer.CategorySerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        return api_models.Category.objects.all()

class PostCategoryListAPIView(generics.ListAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        category_slug = self.kwargs['category_slug'] 
        category = api_models.Category.objects.get(slug=category_slug)
        return api_models.Post.objects.filter(category=category, status="Active")

class PostListAPIView(generics.ListAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        return api_models.Post.objects.all()
    
class PostDetailAPIView(generics.RetrieveAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def get_object(self):
        slug = self.kwargs['slug']
        post = api_models.Post.objects.get(slug=slug, status="Active")
        post.view += 1
        post.save()
        return post
        
class LikePostAPIView(APIView):
    @swagger_auto_schema(
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'user_id': openapi.Schema(type=openapi.TYPE_INTEGER),
                'post_id': openapi.Schema(type=openapi.TYPE_STRING),
            },
        ),
    )
    def post(self, request):
        user_id = request.data['user_id']
        post_id = request.data['post_id']

        user = api_models.User.objects.get(id=user_id)
        post = api_models.Post.objects.get(id=post_id)

        # Check if post has already been liked by this user
        if user in post.likes.all():
            # If liked, unlike post
            post.likes.remove(user)
            return Response({"message": "Post Disliked"}, status=status.HTTP_200_OK)
        else:
            # If post hasn't been liked, like the post by adding user to set of poeple who have liked the post
            post.likes.add(user)
            
            # Create Notification for Author
            api_models.Notification.objects.create(
                user=post.user,
                post=post,
                type="Like",
            )
            return Response({"message": "Post Liked"}, status=status.HTTP_201_CREATED)
        
class PostCommentAPIView(APIView):
    @swagger_auto_schema(
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'post_id': openapi.Schema(type=openapi.TYPE_INTEGER),
                'name': openapi.Schema(type=openapi.TYPE_STRING),
                'email': openapi.Schema(type=openapi.TYPE_STRING),
                'comment': openapi.Schema(type=openapi.TYPE_STRING),
            },
        ),
    )
    def post(self, request):
        # Get data from request.data (frontend)
        post_id = request.data['post_id']
        name = request.data['name']
        email = request.data['email']
        comment = request.data['comment']

        post = api_models.Post.objects.get(id=post_id)

        # Create Comment
        api_models.Comment.objects.create(
            post=post,
            name=name,
            email=email,
            comment=comment,
        )

        # Notification
        api_models.Notification.objects.create(
            user=post.user,
            post=post,
            type="Comment",
        )

        # Return response back to the frontend
        return Response({"message": "Commented Sent"}, status=status.HTTP_201_CREATED)
 
class BookmarkPostAPIView(APIView):
    @swagger_auto_schema(
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'user_id': openapi.Schema(type=openapi.TYPE_INTEGER),
                'post_id': openapi.Schema(type=openapi.TYPE_STRING),
            },
        ),
    )
    
    def post(self, request):
        user_id = request.data['user_id']
        post_id = request.data['post_id']

        user = api_models.User.objects.get(id=user_id)
        post = api_models.Post.objects.get(id=post_id)

        bookmark = api_models.Bookmark.objects.filter(post=post, user=user).first()
        if bookmark:
            # Remove post from bookmark
            bookmark.delete()
            return Response({"message": "Post Un-Bookmarked"}, status=status.HTTP_200_OK)
        else:
            api_models.Bookmark.objects.create(
                user=user,
                post=post
            )

            # Notification
            api_models.Notification.objects.create(
                user=post.user,
                post=post,
                type="Bookmark",
            )
            return Response({"message": "Post Bookmarked"}, status=status.HTTP_201_CREATED)


######################## Author Dashboard APIs ########################
class DashboardStats(generics.ListAPIView):
    serializer_class = api_serializer.AuthorStats
    permission_classes = [AllowAny]

    def get_queryset(self):
        user_id = self.kwargs['user_id']
        user = api_models.User.objects.get(id=user_id)

        views = api_models.Post.objects.filter(user=user).aggregate(view=Sum("view"))['view']
        posts = api_models.Post.objects.filter(user=user).count()
        likes = api_models.Post.objects.filter(user=user).aggregate(total_likes=Sum("likes"))['total_likes']
        bookmarks = api_models.Bookmark.objects.all().count()

        return [{
            "views": views,
            "posts": posts,
            "likes": likes,
            "bookmarks": bookmarks,
        }]
    
    def list(self, request, *args, **kwargs):
        querset = self.get_queryset()
        serializer = self.get_serializer(querset, many=True)
        return Response(serializer.data)

class DashboardPostLists(generics.ListAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        user_id = self.kwargs['user_id']
        user = api_models.User.objects.get(id=user_id)

        return api_models.Post.objects.filter(user=user).order_by("-id")

class DashboardCommentLists(generics.ListAPIView):
    serializer_class = api_serializer.CommentSerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        return api_models.Comment.objects.all()

class DashboardNotificationLists(generics.ListAPIView):
    serializer_class = api_serializer.NotificationSerializer
    permission_classes = [AllowAny]

    def get_queryset(self):
        user_id = self.kwargs['user_id']
        user = api_models.User.objects.get(id=user_id)

        return api_models.Notification.objects.filter(seen=False, user=user)

class DashboardMarkNotiSeenAPIView(APIView):
    @swagger_auto_schema(
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'noti_id': openapi.Schema(type=openapi.TYPE_INTEGER),
            },
        ),
    )
    def post(self, request):
        noti_id = request.data['noti_id']
        noti = api_models.Notification.objects.get(id=noti_id)

        noti.seen = True
        noti.save()

        return Response({"message": "Noti Marked As Seen"}, status=status.HTTP_200_OK)

class DashboardPostCommentAPIView(APIView):
    @swagger_auto_schema(
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'comment_id': openapi.Schema(type=openapi.TYPE_INTEGER),
                'reply': openapi.Schema(type=openapi.TYPE_STRING),
            },
        ),
    )
    def post(self, request):
        comment_id = request.data['comment_id']
        reply = request.data['reply']

        print("comment_id =======", comment_id)
        print("reply ===========", reply)

        comment = api_models.Comment.objects.get(id=comment_id)
        comment.reply = reply
        comment.save()

        return Response({"message": "Comment Response Sent"}, status=status.HTTP_201_CREATED)
    
class DashboardPostCreateAPIView(generics.CreateAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def create(self, request, *args, **kwargs):
        print(request.data)
        user_id = request.data.get('user_id')
        title = request.data.get('title')
        image = request.data.get('image')
        description = request.data.get('description')
        tags = request.data.get('tags')
        category_id = request.data.get('category')
        post_status = request.data.get('post_status')

        print(user_id)
        print(title)
        print(image)
        print(description)
        print(tags)
        print(category_id)
        print(post_status)

        user = api_models.User.objects.get(id=user_id)
        category = api_models.Category.objects.get(id=category_id)

        post = api_models.Post.objects.create(
            user=user,
            title=title,
            image=image,
            description=description,
            tags=tags,
            category=category,
            status=post_status
        )

        return Response({"message": "Post Created Successfully"}, status=status.HTTP_201_CREATED)

class DashboardPostEditAPIView(generics.RetrieveUpdateDestroyAPIView):
    serializer_class = api_serializer.PostSerializer
    permission_classes = [AllowAny]

    def get_object(self):
        user_id = self.kwargs['user_id']
        post_id = self.kwargs['post_id']
        user = api_models.User.objects.get(id=user_id)
        return api_models.Post.objects.get(user=user, id=post_id)

    def update(self, request, *args, **kwargs):
        post_instance = self.get_object()

        title = request.data.get('title')
        image = request.data.get('image')
        description = request.data.get('description')
        tags = request.data.get('tags')
        category_id = request.data.get('category')
        post_status = request.data.get('post_status')

        print(title)
        print(image)
        print(description)
        print(tags)
        print(category_id)
        print(post_status)

        category = api_models.Category.objects.get(id=category_id)

        post_instance.title = title
        if image != "undefined":
            post_instance.image = image
        post_instance.description = description
        post_instance.tags = tags
        post_instance.category = category
        post_instance.status = post_status
        post_instance.save()

        return Response({"message": "Post Updated Successfully"}, status=status.HTTP_200_OK)


{
    "title": "New post",
    "image": "",
    "description": "lorem",
    "tags": "tags, here",
    "category_id": 1,
    "post_status": "Active"
}
```

```python
from django.urls import path
from rest_framework_simplejwt.views import TokenRefreshView
from api import views as api_views

urlpatterns = [
    # Userauths API Endpoints
    path('user/token/', api_views.MyTokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('user/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    path('user/register/', api_views.RegisterView.as_view(), name='auth_register'),
    path('user/profile/<user_id>/', api_views.ProfileView.as_view(), name='user_profile'),
    path('user/password-reset/<email>/', api_views.PasswordEmailVerify.as_view(), name='password_reset'),
    path('user/password-change/', api_views.PasswordChangeView.as_view(), name='password_reset'),

    # Post Endpoints
    path('post/category/list/', api_views.CategoryListAPIView.as_view()),
    path('post/category/posts/<category_slug>/', api_views.PostCategoryListAPIView.as_view()),
    path('post/lists/', api_views.PostListAPIView.as_view()),
    path('post/detail/<slug>/', api_views.PostDetailAPIView.as_view()),
    path('post/like-post/', api_views.LikePostAPIView.as_view()),
    path('post/comment-post/', api_views.PostCommentAPIView.as_view()),
    path('post/bookmark-post/', api_views.BookmarkPostAPIView.as_view()),

    # Dashboard APIS
    path('author/dashboard/stats/<user_id>/', api_views.DashboardStats.as_view()),
    path('author/dashboard/post-list/<user_id>/', api_views.DashboardPostLists.as_view()),
    path('author/dashboard/comment-list/', api_views.DashboardCommentLists.as_view()),
    path('author/dashboard/noti-list/<user_id>/', api_views.DashboardNotificationLists.as_view()),
    path('author/dashboard/noti-mark-seen/', api_views.DashboardMarkNotiSeenAPIView.as_view()),
    path('author/dashboard/reply-comment/', api_views.DashboardPostCommentAPIView.as_view()),
    path('author/dashboard/post-create/', api_views.DashboardPostCreateAPIView.as_view()),
    path('author/dashboard/post-detail/<user_id>/<post_id>/', api_views.DashboardPostEditAPIView.as_view()),


]
```
