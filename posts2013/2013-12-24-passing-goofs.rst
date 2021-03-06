===========================================
Exceptions as Decorator Arguments
===========================================

:date: 2013-12-24 13:15
:tags: python, howto
:category: python
:slug: exceptions-as-decorator-arguments

I wanted to see if I could have an exception as a decorator argument. Here is what I came up with:

.. code-block:: python

    import functools

    class MyGoof(Exception):
        pass

    def pass_goof(exception):
        def decorator(test_func):
            @functools.wraps(test_func)
            def wrapper(*args, **kwargs):
                try:
                    return test_func(*args, **kwargs)
                except exception as e:
                    return None
            return wrapper
        return decorator

    @pass_goof(MyGoof)
    def test1():
        return "test1"
    
    @pass_goof(MyGoof)
    def test2():
        raise MyGoof
    
    assert test1() == 'test1'
    assert test2() == None
