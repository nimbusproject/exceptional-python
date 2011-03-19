# pylons-exceptional

`pylons-exceptional` is a pylons client for [Exceptional][], a service which
tracks errors in your web apps.

  [exceptional]: http://getexceptional.com/

It is a direct [adaptation][] from `django-exceptional`.

  [adaptation]: https://github.com/zacharyvoase/django-exceptional

## Get started in 3 steps

Install the app:
    easy_install pylons-exceptional
or
    pip install pylons-exceptional

Store your API key in your application configuration:
    # GetExceptional config
    exceptional.root=%(here)s
    exceptional.api_key=a_really_long_hex_key

Configure the new error handler in your application (`config/middleware.py`):

    if asbool(full_stack):
        # Display error documents for 401, 403, 404 status codes (and
        # 500 when debug is disabled)
        if asbool(config['debug']):
            app = ErrorHandler(app, global_conf, **config['pylons.errorware'])
            app = StatusCodeRedirect(app)
        else:
            # Send error to getexceptional if we can
            try:
                from exceptional import ExceptionalMiddleware
                app = ExceptionalMiddleware(app, config['exceptional.api_key'])
            except:
                import sys
                print "ERROR: Cannot setup exceptional error reporting", sys.exc_info()
                app = ErrorHandler(app, global_conf, **config['pylons.errorware'])
            app = StatusCodeRedirect(app, [400, 401, 403, 404, 500])

You are done.

In this configuration `getexceptional` will only be triggered if you *don't*
run in debug mode.

## (Un)license

This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <http://unlicense.org/>
