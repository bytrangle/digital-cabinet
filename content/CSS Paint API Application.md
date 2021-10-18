# CSS Paint API Application
[View this newsletter on the web.](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=1d26c658ac&e=2df64611c3)

### ![](https://fonts.gstatic.com/s/e/notoemoji/13.1.1/270c_fe0f/32.png)

![](https://ci4.googleusercontent.com/proxy/PhW6MqUiXZJUocC-KLBZARUSShM1B-0izq3UN6pHe9mGd_hhsR-fDXnySy-K9iQVKBgjtWojr8qxtjtZUAeACeybPDRAsCuQCj1tv9HaPvgI_DFuE8Z2H44fo7ecg937UPtRz7SoKSW3cyNpDKUDlWEM=s0-d-e1-ft#https://i1.wp.com/css-tricks.com/wp-content/uploads/2021/08/2021-08-15-16.52.07.gif?w=560&ssl=1)

See that nifty hover effect up there? That’s from a post Temani Afif wrote about how to use the CSS Paint API to create an [image fragmentation effect](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=e9b09bad0c&e=2df64611c3). It’s _extremely_ neat:

> The [Paint API](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=48dc11325e&e=2df64611c3) is part of the Houdini project. Yes, “Houdini” the strange term that everyone is talking about. [A lot of articles](https://css-tricks.us2.list-manage.com/track/click?u=155f5a9ccc4e24c318130cace&id=a92c8cea65&e=2df64611c3) already cover the theoretical aspect of it, so I won’t bother you with more. If I have to sum it up in a few words, I would simply say : **it’s the future of CSS**. The Paint API (and the other APIs that fall under the Houdini umbrella) allow us to extend CSS with our own functionalities. We no longer need to wait for the release of new features because we can do it ourselves!

And just look at the CSS for this thing!

    img {
      -webkit-mask: paint(fragmentation);
      --f-o: 1;
      transition:--f-o 1s;
    }

    img:hover {
      --f-o: 0;
    }

This is how you apply the Paint API to an HTML element — and I’m skipping out all the JavaScript here — but if I saw CSS like this a few years ago I would have thought it’s from an alien language, not the CSS I know and love.

* * *

 [https://mail.google.com/mail/u/0/#inbox/FMfcgzGkZsrWNtttnqrhCjHVPBMdSjWP](https://mail.google.com/mail/u/0/#inbox/FMfcgzGkZsrWNtttnqrhCjHVPBMdSjWP)
