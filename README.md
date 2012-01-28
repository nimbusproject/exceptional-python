# exceptional-python

`exceptional-python` is a python client for [Exceptional][], a service which
tracks errors in your web apps.

  [exceptional]: http://getexceptional.com

It is adapted from `pylons-exceptional` by removing dependencies to `pylons`.

## Usage

Send exception directly

    exceptional = Exceptional('YOUR_API_KEY_HERE')
    try:
      1/0
    except Exception as e:
      exceptional.submit(e, os.environ)
      raise

or, use log handler

    import logging
    logger = logging.getLogger(__name__)
    handler = ExceptionalLogHandler('YOUR_API_KEY_HERE')
    handler.setLevel(logging.ERROR)
    logger.addHandler(handler)
    
    try:
      1/0
    except:
      logger.error('oops!')
