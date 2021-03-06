**phpFastCache** is a high-performance, distributed object caching system, generic in nature, but intended for use in speeding up dynamic web applications by alleviating database load.

**FastCache** is built on top of **phpFastCache**. Its usage is very simple:

````php
use Helpers\FastCache;

$cache = FastCache::getInstance();

// Use any phpFastCache methods on FastCache instance.
```

To note that the **Helpers\FastCache** acts like a Decorator for **phpFastCache**, it can call any **phpFastCache** method. For full documentation on **phpFastCache** see the website http://www.phpfastcache.com