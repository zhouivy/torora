# Introduction #

[This page on the WebKit wiki](https://trac.webkit.org/wiki/Fingerprinting) is the principal reference point here. That document is based on [DESIGN.TORORA](https://github.com/mwenge/torora/blob/master/doc/DESIGN.torora).

# Good General Tests for Assessing Protection Provided by Torora #

http://samy.pl/evercookie/

http://decloak.net/

# Known Attack Vectors #

## Javascript ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  `Document.referrer` | Y            | **N**     | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#i.DocumentObject) | http://browserspy.dk/document.php |
|  `history.length` | Y            | **N**     | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#ii.Window.HistoryObject) | http://browserspy.dk/document.php |
|  `Window`         | Y            | [Imperfect](http://code.google.com/p/torora/issues/detail?id=9) | On Linux, browser resizes to round values. No countermeasures on Windows currently. | [info](https://trac.webkit.org/wiki/Fingerprinting#iii.Windowobject) | http://browserspy.dk/window.php |
|  `Window.name`    | Y            | Y         | At least git revision da17daf | [info](https://trac.webkit.org/wiki/Fingerprinting#iv.Window.name) | http://roberthogan.net/stuff/window.name/sessvarsTestPage1.html |
|  `Screen`         | Y            | Y         | At least git revision 2e5d3cb. Requires WebKit trunk. | [info](https://trac.webkit.org/wiki/Fingerprinting#v.Screenobject) | http://browserspy.dk/screen.php |
|  `Navigator`      | Y            | Y         | At least git revision 2e5d3cb Requires WebKit trunk. | [info](https://trac.webkit.org/wiki/Fingerprinting#vi.NavigatorObject) | http://browserspy.dk/browser.php |
|  `Date` and Timezone | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a.Timezone) | http://browserspy.dk/date.php |
|  `Date` and Typing Cadence | N            | N         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#b.TimingUsers) | None Available |
|  `Language`       | ?            | ?         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#viii.LanguageObject) |  http://browserspy.dk/language.php |

## CSS ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  CSS Media Queries | N            | N         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#i.CSSMediaQueries) | None available |
|  CSS Fonts Introspection | N            | N         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#ii.CSSFonts) | http://flippingtypical.com/ |
|  CSS History Inspection | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#iii.QueryingPageHistorywithCSS) | http://browserspy.dk/css-exploit.php |


## HTTP Headers ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  User Agent Header | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a11.HTTPHeaders) | http://browserspy.dk/headers.php |
|  Referer and Origin Header | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a11.HTTPHeaders) | http://browserspy.dk/headers.php |
|  Accept-Language Header | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a11.HTTPHeaders) | http://browserspy.dk/language.php |
|  Accept Header    | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a11.HTTPHeaders) | http://browserspy.dk/headers.php |
|  HTTP ETags       | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a11.HTTPHeaders) | [https://trac.webkit.org/wiki/Fingerprinting#v.HTTPETags |
|  'Do Not Track'   | ?            | Y         | At least git revision d0d234de | [info](http://datatracker.ietf.org/doc/draft-mayer-do-not-track) | http://browserspy.dk/donottrack.php |

## Plugins/Java ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  `Navigator.plugins` | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#InstalledPlugins) | N/A      |
|  Plugins Disabled | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#InstalledPlugins) | N/A      |
|  Java Disabled    | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#InstalledPlugins) | N/A      |


## Storage ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  Cookies          | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a8.Cookies) | http://browserspy.dk/cookie.php http://evercookie.org |
|  Third Party Cookies | Y            | **N**     | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#a9.ThirdPartyCookies) | http://browserspy.dk/cookie.php |
|  Page Cache       | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a10.PageCache) | http://evercookie.org |
|  DOM Local Storage | Y            | Y         | At least git revision da17daf  | [info](https://trac.webkit.org/wiki/Fingerprinting#a12.DOMLocalStorageDOMSessionStorageDOMGlobalStorage) | N/A      |
|  DOM Global Storage | Y            | Y         | At least git revision da17daf  | [info](https://trac.webkit.org/wiki/Fingerprinting#a12.DOMLocalStorageDOMSessionStorageDOMGlobalStorage) | N/A      |
|  DOM Session Storage | Y            | Y         | At least git revision da17daf  | [info](https://trac.webkit.org/wiki/Fingerprinting#a12.DOMLocalStorageDOMSessionStorageDOMGlobalStorage) | N/A      |

## TLS/SSL Session IDs ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
|  TLS/SSL Session IDs | Y            | ?         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#SessionIDs) | N/A      |

## Fonts Introspection ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
| Fonts Introspection | N            | N         | N/A       | [info](https://trac.webkit.org/wiki/Fingerprinting#Fonts) | N/A      |

## Geolocation ##
| **Attack Vector** | **Torbutton**| **Torora**| **Since** |  **Info** | **Test** |
|:------------------|:-------------|:----------|:----------|:----------|:---------|
| Geolocation       | Y            | Y         | At least git revision 2e5d3cb | [info](https://trac.webkit.org/wiki/Fingerprinting#a15.GeoLocation) | http://browserspy.dk/geolocation.php |