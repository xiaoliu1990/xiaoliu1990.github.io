# js计算周

```javascript
  function formatDig(num) {
    return num > 9 ? '' + num : '0' + num;
  }

  function formatDate(mill) {
    var y = new Date(mill);
    let raws = [
      y.getFullYear(),
      formatDig(y.getMonth() + 1),
      formatDig(y.getDate()),
      y.getDay() || 7
    ];
    let format = ['年', '月', '日 星期'];
    return String.raw({
      raw: raws
    }, ...format);
  }

  function* createWeeks(year) {
    const ONE_DAY = 24 * 3600 * 1000;
    let start = new Date(year, 0, 1),
      end = new Date(year, 11, 31);
    let firstDay = start.getDay() || 7,
      lastDay = end.getDay() || 7;
    let startTime = +start,
      endTime = startTime + (7 - firstDay) * ONE_DAY,
      _endTime = end - (7 - lastDay) * ONE_DAY;
    yield [startTime, endTime];
    startTime = endTime + ONE_DAY;
    endTime = endTime + 7 * ONE_DAY;
    while (endTime < _endTime) {
      yield [startTime, endTime];
      startTime = endTime + ONE_DAY;
      endTime = endTime + 7 * ONE_DAY;
    }
    yield [startTime, +end];
  }
  let index = 1;
  for (let i of createWeeks(2019)) {
    let start = i[0],
      end = i[1];
    console.log(`第${formatDig(index++)}周 ${formatDate(start)}-${formatDate(end)}`);
  }
```
