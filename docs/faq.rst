FAQ
===


I see an error saying "could not import myproject.settings"
-----------------------------------------------------------

py.test does not automatically add your project to the Python path, that is
usually done when running tests via `manage.py test`.

The easiest way to get your project code available on the Python path is to
create a `setup.py` like this::

    from distutils.core import setup
    setup(name='myproj', version='1.0')

And then install your project into your virtualenv with ``pip install -e .``.

Alternatively, when you create a `conftest.py` file in your project root, it
will also implicitly add that directory to the python path.

How can I make sure that all my tests run with a specific locale?
-----------------------------------------------------------------

Activate a specific locale in your project's ``conftest.py``::

    from django.utils.translation import activate

    def pytest_runtest_setup(item):
        activate('en')

.. _faq-tests-not-being-picked-up:

My tests are not being picked up when I run py.test from the root directory. Why not?
-------------------------------------------------------------------------------------
 By default, py.test looks for tests in files named ``test_*.py`` (note that this is not the same as ``test*.py``). 
 If you have your tests in files with other names, they will not be collected. It is common to put tests under
 ``app_directory/tests/views.py``. To find those tests, create a ``pytest.ini`` file in your
 project root with the contents::

    [pytest]
    python_files=*.py


.. _faq-django-settings-module:

How can I avoid having to type DJANGO_SETTINGS_MODULE=... to run the tests?
---------------------------------------------------------------------------

If you are using virtualenvwrapper, use a postactivate script to set ``DJANGO_SETTINGS_MODULE`` when your project's virtualenv is activated.

This snippet should do the trick (make sure to replace ``YOUR_VIRTUALENV_NAME``)::

    echo "export DJANGO_SETTINGS_MODULE=yourproject.settings" >> $WORKON_HOME/YOUR_VIRTUALENV_NAME/bin/postactivate


How does South and pytest-django play together?
------------------------------------------------

Djangos own syncdb will always be used to create the test database, regardless of wheter South is present or not.


Does this work with the pytest-xdist plugin?
--------------------------------------------

Yes, pytest-django supports running tests in parallel with pytest-xdist. Each
process created by xdist gets its own database that is used for the tests. This
ensures that each test can run independently.
