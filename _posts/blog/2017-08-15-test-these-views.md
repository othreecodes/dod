---
layout: post
title: Test these views
excerpt: "My first day working physically after a couple of months of working remotely. Here's what I learnt"
categories: blog
tags: [first-day, code, django, unit tests, python, REST]
comments: true
share: true
modified: 2017-08-15T22:01:34.637579
---

Soo...
Where I work, (As a Back-end Developer (Soon to be Full stack)), We've been working on changing the site architecture from 
a monolith (hope i spelt that right...im too lazy to google it) to a Microservice-y architecture.

Basically these means that we'll be breaking down the site into services that communicate to eachother via APIs.(Its mainly [Django](http://djangoproject.com) bdw)
Now I'm working on this service, pulling trying to replace the functionality thats in the current code base in my own microservice.
Before today, this was my REST view using [Django rest framework (DRF)](http://djangorestframework)

*NB: If you haven't heard of Django before, the rest of these stuff will be to you what PHP is to me*

```python

#A Simple FBV (function based view)
@api_view(http_method_names=['GET'])
def single_category(request, slug):

    #This shii is in the models (does some DB manipulation Yada yada)
    data = cached_category_skills(request.user, slug)

    return Response(data=data, status=200)


#and my CBV (class based view)

class Categories(generics.ListAPIView):
    model = Category
    serializer_class = CategorySerializer
    queryset = model.objects.all()
```

Straight forward yh? Its all good. Then I'll hook 'em up to the urls like this

```python
    url(r'^skill/categories/$', view=skill_views.Categories.as_view(), name="categories"),
    url(r'^skill/categories/(?P<slug>[\w.@+-]+)', view=skill_views.single_category, name="category"),
```

This works... but little did I know that there was a simpler way to do this using [DRF](http://djangorestframework)
Introducing....[router](http://www.django-rest-framework.org/api-guide/routers/) and [viewsets](http://www.django-rest-framework.org/api-guide/viewsets/)...

A `ViewSet` class is simply a type of class-based View, that does not provide any method handlers such as `.get()` or `.post()`, and instead provides actions such as `.list()` and `.create().`

A router is just a quick and consistent way of wiring your view logic to a set of URLs.

```python

class CategoriesViewSet(viewsets.ModelViewSet):
    serializer_class = CategorySerializer
    lookup_field = "slug"

    def get_queryset(self):
        return Category.objects.all()

    def retrieve(self, request, **kwargs):

        instance = self.get_object()

        serializer = SkillSerializer(instance.skill_set.all(), many=True)
        return Response(serializer.data)


router = routers.DefaultRouter()

router.register(prefix=r'categories', viewset=CategoriesViewSet,
                base_name="fetch_categories")

```
**I'm not the official documentation so don't expect me to gist you on what exactly this does**

Then in my urls.py 
```python
 url(r'^', include(views.router.urls)),
 #Dassal
```
A lotta abstraction is done by the ModelViewSet class. Check this out.

```python
class ModelViewSet(mixins.CreateModelMixin,
                   mixins.RetrieveModelMixin,
                   mixins.UpdateModelMixin,
                   mixins.DestroyModelMixin,
                   mixins.ListModelMixin,
                   GenericViewSet):
    """
    A viewset that provides default `create()`, `retrieve()`, `update()`,
    `partial_update()`, `destroy()` and `list()` actions.
    """
    pass

```

> Just Imagine the possibilities read the [docs](http://www.django-rest-framework.org/api-guide/viewsets/) you'll love it too
A lotta stuff that i woulda done manually has already been done by the ModelViewSet class. Including [browsable API shit](http://www.django-rest-framework.org/topics/browsable-api/)

[![Browsable API](/images/browsableapi.png)](https://youtu.be/vXpWvU9HKZs?t=3 "E don do me")


# Now to the testing.....
Damn... The funniest thing about unit tests in django is that the name of the test function must start with `test`
unless the test wouldn't run e.g 

```python

class BaseTestCase(TestCase):

    def test_price_of_pure_water_will_return_to_five_naira(self):
        pass

```

This stupid thing kept me up one night. Bdw I use `django-test-plus` for testing, cool library you should check it out.
It provides a lot of method that the default django test doesn't.

So apparently, I learnt from the CTO (cool guy) today that your test for the views should also test the actual expected result, so here's a sample of one
of my unit tests

```python
def test_quiz_questions_are_fecthed_for_quiz(self):
        response = self.client.get(
            reverse('quiz:fetch_questions-detail', kwargs={'url': self.skill1.quiz.url}))

        data = response.json()
        self.response_200(response)

        self.assertEqual(len(data), 1)

        self.assertEqual(data, [{'answer_set': [{'content': '', 'correct': False, 'id': 168}, {'content': '', 'correct': False, 'id': 167}, {
                         'content': '', 'correct': True, 'id': 166}, {'content': '', 'correct': False, 'id': 165}], 'content': 'whats my name?', 'id': 0}])

``` 

I couldn't be more explicit ;)

I usually first test for a `200 response` before i test the data... Im just used to It. 
The `self.client.get` comes from `django-test-plus`

Finally, theres also this cool library `django-extensions` check it on github, I've been using it for a while, but didnt know about the management commands
including

```bash
$ python manage.py shell_plus
```

and 


```bash
$ python manage.py show_urls
```

Damn good stuff

`shell_plus` gives some kinda Ipython interface while `show_urls` shows you the urls in your project and the views the point to

here are a list of other stuff that exist

```bash
[django_extensions]
    admin_generator
    clean_pyc
    clear_cache
    compile_pyc
    create_app
    create_command
    create_jobs
    create_template_tags
    delete_squashed_migrations
    describe_form
    drop_test_database
    dumpscript
    export_emails
    find_template
    generate_secret_key
    graph_models
    mail_debug
    notes
    passwd
    pipchecker
    print_settings
    print_user_for_session
    reset_db
    runjob
    runjobs
    runprofileserver
    runscript
    runserver_plus
    set_default_site
    set_fake_emails
    set_fake_passwords
    shell_plus
    show_template_tags
    show_templatetags
    show_urls
    sqlcreate
    sqldiff
    sqldsn
    sync_s3
    syncdata
    unreferenced_files
    update_permissions
    validate_templates
```


Really cool stuff...

Now I'm off to more debugging.

PS: check out this [Wakatime extension](https://wakatime.com) tracks the amount of time you spend coding on your text editor.