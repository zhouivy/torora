# It kind of already does. #

This is thanks to the [Necessitas SDK](http://sourceforge.net/p/necessitas/home/).

We have an [android branch](https://github.com/mwenge/torora/tree/android).

We even have visual proof that it can run:

![http://roberthogan.net/images/torora-android.png](http://roberthogan.net/images/torora-android.png)

# However, there are a couple of things blocking: #

[Android's CA cert bundle is not compatible with Qt](http://sourceforge.net/p/necessitas/tickets/27/) and there is no obvious or easy way to fix that without Ministro/Necessitas or Torora shipping a certificate bundle of its own.