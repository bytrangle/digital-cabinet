# How Terra improved user engagement thanks to Dark Mode
-   [Home](https://web.dev/)
-   [All articles](https://web.dev/blog)

By displaying a dark theme to users that prefer dark mode on their devices, Terra reduced the bounce rate by 60% and increased the pages read per session by 170%.

Dec 18, 2021

[![](https://web-dev.imgix.net/image/admin/XVGMhdOgHJhch3EBcw89.jpg?auto=format&fit=crop&h=64&w=64)
](https://web.dev/authors/andreban/)

[![](https://web-dev.imgix.net/image/26V1DWN36MZr3mUo8ChSBlCpzp43/wYWiAqwJid6lXBpotD6h.jpeg?auto=format&fit=crop&h=64&w=64)
](https://web.dev/authors/mobtec/)

Terra, one of Brazil's largest media companies with 75 million monthly users, reduced the bounce rate by 60% and increased the pages read per session by 170% on desktop for users that prefer dark mode by providing a custom dark theme.

In this article, we'll analyze Terra's journey from identifying the size of the "dark mode" cohort, to applying a custom dark theme, and finally measuring the impact of this implementation on their main KPIs.

60%

Reduction in Bounce Rates

170%

More pages per session

## What is dark mode? [#](#what-is-dark-mode)

Historically user interfaces in devices are displayed in "light mode", which usually means displaying black text on top of light interfaces. The alternative is "dark mode", with light text on a dark background, such as gray or black.

Dark Mode has [benefits](https://web.dev/prefers-color-scheme/#why-dark-mode) for user experience. Some people prefer it for aesthetic or accessibility reasons. It has other advantages, such as preserving battery life in devices. Users can express that they prefer dark mode via a setting in their devices, [which depends on the operating system](https://web.dev/prefers-color-scheme/#activating-dark-mode-in-the-operating-system). For example, the following screenshot shows what the **Dark Theme** configuration option looks like in devices that run **Android Q**:

![](https://web-dev.imgix.net/image/admin/Yh6SEoWDK1SbqcGjlL6d.png?auto=format)

Android Q dark theme settings.

To offer a better experience to users who prefer "dark mode", you can use the [`prefers-color-scheme`](https://developer.mozilla.org/docs/Web/CSS/@media/prefers-color-scheme) media query, with a value of `light` or `dark`. This media query reflects the user's choice in their device. You can then load a [custom dark theme](https://web.dev/prefers-color-scheme/#dark-mode-in-practice) for those that prefer dark. For example, by loading a "dark" CSS file, and changing values such as font and background colors. The following code shows an example of that:

```
@media (prefers-color-scheme: dark) {  
   // apply a dark theme  
}

@media (prefers-color-scheme: light) {  
  // apply a light theme  
}


```

Browser support: chrome 76, Supported 76 firefox 67, Supported 67 edge 79, Supported 79 safari 12.1, Supported 12.1

[Source](https://developer.mozilla.org/docs/Web/CSS/@media/prefers-color-scheme#browser_compatibility)

## Identifying the "prefers light" vs "dark" user cohorts [#](#identifying-the-"prefers-light"-vs-"dark"-user-cohorts)

At the time of writing (December 2021), [Chrome Platform Status](https://chromestatus.com/features) determines that approximately [22% of the web traffic](https://chromestatus.com/metrics/feature/timeline/popularity/3581) comes from users with the "prefer dark" setting.

This is aggregated data, so the real percentage of users who prefer dark that come to a site can vary. For that reason, to understand the size of this group it is advisable to run in house measurement.

The following code creates an analytics dimension, to measure the performance of users that prefer `light` vs. `dark`:

```
function getColorScheme() {  
    let colorScheme = 'Unknown';  
    if (window.matchMedia) {  
    if (window.matchMedia('(prefers-color-scheme: dark)').matches) {  
        colorScheme = 'Dark';  
    } else if (window.matchMedia('(prefers-color-scheme: light)').matches) {   colorScheme = 'Light'; }  
    }  
    return colorScheme;  
}

window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};  
ga.l=+new Date; ga('create', 'UA-ID', 'auto');  
ga('set', 'color-scheme-preference', getColorScheme());  
ga('send', 'pageview');


```

Terra implemented this code in their site and compared the behavior of both groups in mobile (Android) and desktop (Windows) devices. At that moment Terra wasn't providing a custom dark theme, so the goals of this experiment were:

-   Determining the size of the group of users who prefer dark.
-   Identifying patterns: for example, do users that prefer dark leave the site more quickly compared to those that prefer light?

Knowing this can inform decisions, for example: if it's necessary to provide a custom dark theme. These are the results Terra obtained after testing for 14 days:

### Mobile (Android) [#](#mobile-(android))

In the case of mobile (Android) the numbers for bounce rate and pages per session didn't show big differences between the users that prefer"light", compared to those that prefer"dark":

### Desktop (Windows) [#](#desktop-(windows))

In the case of desktop (Windows), the group of users that preferred "dark" stayed much less on the site: they had almost **twice the bounce rate and read a little more than half of the pages** than those users that preferred "light":

Based on this data, Terra hypothesized that users who prefer "dark" stay less in desktop devices, due to the lack of support of a dark theme in their site.

As a next step Terra decided to work on a "dark theme" strategy to see if they could improve the engagement for the group of users that preferred dark.

## Implementing a dark theme [#](#implementing-a-dark-theme)

Most websites implement a dark theme by using the simple strategy shown previously of listening to user's configuration changes via the [`prefers-color-scheme`](https://drafts.csswg.org/mediaqueries-5/#prefers-color-scheme) media query and changing styles based on that.

Terra decided to give more control to the user: when they detect that they have the "prefer dark" setting turned on in their devices (via the media query), they show them a prompt to ask them if they want to navigate in "night mode". As long as the user doesn't deny the prompt (by clicking on the"Ignore"button), they honor the user's OS-setting, and apply a custom dark theme:

![](https://web-dev.imgix.net/image/26V1DWN36MZr3mUo8ChSBlCpzp43/TRqfCAmBe025456JyX1b.png?auto=format)

Terra shows a prompt to the user asking if they want to navigate in dark mode after detecting that they prefer dark in their devices.

As a complement of this strategy they provide additional configuration options in the "settings" screen, where the user can decide if they explicitly prefer "light", "dark", or want to rely on the underlying device settings.

![](https://web-dev.imgix.net/image/26V1DWN36MZr3mUo8ChSBlCpzp43/B7g0uvq2QB0eWVjnuMAl.png?auto=format)

Terra's themes configurations allow users to choose between"Dark"and"Light"themes or rely on the device's settings.

This is how Terra's"Night Mode" looks like:

![](https://web-dev.imgix.net/image/26V1DWN36MZr3mUo8ChSBlCpzp43/QRW06FYeMghUI8obAQWC.png?auto=format)

Terra's dark theme,"Night Mode".

## Measuring the impact of Terra's dark theme [#](#measuring-the-impact-of-terra's-dark-theme)

To measure the impact of the dark theme, Terra took the group of users that have the "Prefer Dark" setting turned on in their devices and compared engagement metrics when showing a "Light" vs. a "DarK" theme. Here are the results for mobile (Android) and desktop (Windows):

### Mobile (Android) [#](#mobile-(android)-2)

While bounce rates remained similar, pages and sessions almost doubled (from 2.47 to 5.24) when users were exposed to a dark theme:

### Desktop (Windows) [#](#desktop-(windows)-2)

Both metrics improved when showing a dark theme: bounce rates went from 27.25% to 10.82% and pages per session almost tripled (from 3.7 to 9.99).

60%

Reduction in Bounce Rates

170%

More pages per session

Based on this data, Terra could confirm the benefits for the users from implementing a dark theme, and has decided to continue maintaining it as a core feature of the site.

## Conclusion [#](#conclusion)

Dark Mode is a preference that users can turn on in their devices to change the style of the screens into dark themes. This technique has reported benefits from the user experience and for different aspects of the user's devices such as saving battery life.

In this article we saw how providing a custom dark theme improved engagement metrics for the group of Terra's users that have the preferred dark mode setting turned on in their devices.

For companies with the resources to implement an alternative dark theme this is the recommended approach. For those that don't have the time to invest in such a feature, Chrome is starting to roll out an [Auto Dark Mode feature](https://developer.chrome.com/blog/auto-dark-theme/).

Last updated: Dec 18, 2021 — [Improve article](https://github.com/GoogleChrome/web.dev/blob/main/src/site/content/en/blog/terra-dark-mode/index.md) 
 [https://web.dev/terra-dark-mode/](https://web.dev/terra-dark-mode/)
