# Timezone

## Time Zone != Time Offset

Time zone is not equal to time offset. Time zone is more like a geological zone that has same time offsets. A time zone can have multiple offsets because of daylight saving time. Even if two regions share a same set of offsets, periods of day of daylight saving time can still be different.

### Example

A website that shows messages to customers. Country managers need to be able to see and edit periods of messages in the local time of their countries. They should be able to use the system in any physical location. A country manager may edit messages for Finland (+02:00) in Germany (+01:00).

- Message edit form shows local time and save it in UTC or time with offset
- Message list shows local time

## What does a reasonable time picker look like?

On 25 March, 2018, the next minute of 01:59:59 CET is 03:00:00 CEST. On 28 October, 2018, the next minute of 02:59:59 CEST is 02:00:00 CET.

It's possible to convert 02:00:00 CET to 03:00:00 CEST on 25 March, 2018. However, 02:00:00 on 28 October, 2018 can be CEST or CET.

One possible idea is to use the offset of the beginning of the day. CET on 25 March, 2018. CEST on 28 October, 2018. 28 October has from 00:00 to 24:59:59, which may complicate the implementation of the time picker.

Another idea is to allow users to select an offset, like CET or CEST, on days when daylight saving time begins or ends. This is more explicit and doesn't involve weird time like 24:59:59.

## Handling Time Zone

If you only handle time zones without DST, it's not that hard. You can just add or subtract time zone offsets of the time zones. However, the calculation of DST makes it much harder. To do it correctly, you need to have a database of time zones and their periods of DST.

As long as you manipulate time in the same zone as user's locale, JavaScript's `Date` handles conversion with UTC and daylight saving time correctly.

However, if you want to manipulate a time in a different time zone as user's locale, it becomes harder. Date's behavior changes according to the user's locale. You cannot specify which time zone to use.

## Time Zone Database

If you want to keep a time zone with a `Date`, you need to keep it outside of Date.

- JavaScript
  - https://momentjs.com/timezone/
  - http://moment.github.io/luxon/
  - http://moment.github.io/luxon/docs/manual/zones.html
- Elm
  - http://package.elm-lang.org/packages/Bogdanp/elm-time

## Abuse Intl

Luxon abuses Intl formatter to get timezone offset of any valid time zone in a pretty smart way. https://github.com/moment/luxon/blob/master/src/zones/IANAZone.js#L102-L112

Intl polyfill doesn't support timezones except UTC https://github.com/andyearnshaw/Intl.js/blob/master/src/12.datetimeformat.js#L195-L198

## Date as a time zone free container

Non-UTC interface is affected by browser's locale. If you use it for a time picker for a time zone different from browser's locale, it may cause a problem.

```js
> new Date(2018, 2, 25, 2, 0, 0)
Sun Mar 25 2018 03:00:00 GMT+0200 (CEST)
> new Date(2018, 2, 25, 2, 0, 0).getHours()
3
```

So time pickers should use UTC interface. Then it can be used as a time zone free container.

```js
> new Date(Date.UTC(2018, 2, 25, 2, 0, 0))
Sun Mar 25 2018 04:00:00 GMT+0200 (CEST)
> new Date(Date.UTC(2018, 2, 25, 2, 0, 0)).getUTCHours()
2
```
