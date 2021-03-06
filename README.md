# humanize

[![PyPI version](https://img.shields.io/pypi/v/humanize.svg?logo=pypi&logoColor=FFE873)](https://pypi.org/project/humanize/)
[![Supported Python versions](https://img.shields.io/pypi/pyversions/humanize.svg?logo=python&logoColor=FFE873)](https://pypi.org/project/humanize/)
[![PyPI downloads](https://img.shields.io/pypi/dm/humanize.svg)](https://pypistats.org/packages/humanize)
[![Travis CI Status](https://travis-ci.com/hugovk/humanize.svg?branch=master)](https://travis-ci.com/hugovk/humanize)
[![GitHub Actions status](https://github.com/jmoiron/humanize/workflows/Test/badge.svg)](https://github.com/jmoiron/humanize/actions)
[![codecov](https://codecov.io/gh/hugovk/humanize/branch/master/graph/badge.svg)](https://codecov.io/gh/hugovk/humanize)
[![MIT License](https://img.shields.io/github/license/jmoiron/humanize.svg)](LICENSE)

This modest package contains various common humanization utilities, like turning
a number into a fuzzy human readable duration ('3 minutes ago') or into a human
readable size or throughput.  It works with Python 2.7 and 3.5+ and is localized
to:

* Dutch
* Finnish
* French
* German
* Indonesian
* Italian
* Korean
* Portuguese Brazilian
* Russian
* Simplified Chinese
* Slovak
* Turkish
* Vietnamese

## Usage

Integer humanization:

```pycon
>>> import humanize
>>> humanize.intcomma(12345)
'12,345'
>>> humanize.intword(123455913)
'123.5 million'
>>> humanize.intword(12345591313)
'12.3 billion'
>>> humanize.apnumber(4)
'four'
>>> humanize.apnumber(41)
'41'
```

Date & time humanization:

```pycon
>>> import datetime
>>> humanize.naturalday(datetime.datetime.now())
'today'
>>> humanize.naturaldelta(datetime.timedelta(seconds=1001))
'16 minutes'
>>> humanize.naturalday(datetime.datetime.now() - datetime.timedelta(days=1))
'yesterday'
>>> humanize.naturalday(datetime.date(2007, 6, 5))
'Jun 05'
>>> humanize.naturaldate(datetime.date(2007, 6, 5))
'Jun 05 2007'
>>> humanize.naturaltime(datetime.datetime.now() - datetime.timedelta(seconds=1))
'a second ago'
>>> humanize.naturaltime(datetime.datetime.now() - datetime.timedelta(seconds=3600))
'an hour ago'
```

File size humanization:

```pycon
>>> humanize.naturalsize(1000000)
'1.0 MB'
>>> humanize.naturalsize(1000000, binary=True)
'976.6 KiB'
>>> humanize.naturalsize(1000000, gnu=True)
'976.6K'
```

Human readable floating point numbers:

```pycon
>>> humanize.fractional(1/3)
'1/3'
>>> humanize.fractional(1.5)
'1 1/2'
>>> humanize.fractional(0.3)
'3/10'
>>> humanize.fractional(0.333)
'1/3'
>>> humanize.fractional(1)
'1'
```

## Localization

How to change locale at runtime:

```pycon
>>> print(humanize.naturaltime(datetime.timedelta(seconds=3)))
3 seconds ago
>>> _t = humanize.i18n.activate('ru_RU')
>>> print(humanize.naturaltime(datetime.timedelta(seconds=3)))
3 секунды назад
>>> humanize.i18n.deactivate()
>>> print(humanize.naturaltime(datetime.timedelta(seconds=3)))
3 seconds ago
```

You can pass additional parameter `path` to `activate` to specify a path to search
locales in.

```pycon
>>> humanize.i18n.activate("pt_BR")
IOError: [Errno 2] No translation file found for domain: 'humanize'
>>> humanize.i18n.activate("pt_BR", path="path/to/my/portuguese/translation/")
<gettext.GNUTranslations instance ...>
```

How to add new phrases to existing locale files:

```console
$ xgettext -o humanize.pot -k'_' -k'N_' -k'P_:1c,2' -l python humanize/*.py  # extract new phrases
$ msgmerge -U humanize/locale/ru_RU/LC_MESSAGES/humanize.po humanize.pot # add them to locale files
$ msgfmt --check -o humanize/locale/ru_RU/LC_MESSAGES/humanize{.mo,.po} # compile to binary .mo
```

How to add a new locale:

```console
$ msginit -i humanize.pot -o humanize/locale/<locale name>/LC_MESSAGES/humanize.po --locale <locale name>
```

Where `<locale name>` is a locale abbreviation, eg. `en_GB`, `pt_BR` or just `ru`, `fr`
etc.
