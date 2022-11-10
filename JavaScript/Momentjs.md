# Moment.js

### 현재 장소 기준 시간 읽기

```
moment();
```

<br />
<br />

### 특정 날짜 이후로 조정하기

moment().add(숫자, 단위)를 사용하여 조정한다.

```
// 2022-11-11T13:46:53+09:00 기준

const momentDate = moment();
// 2022-11-18T13:46:53+09:00

const newMomentDate = momentDate.add(1, "week");
// 2022-11-18T13:46:53+09:00

const cloneNewMomentDate = newMomentDate.clone().add(1, "week");
// 2022-11-24T13:46:53+09:00

```

이전 날씨에 덮어 씌워지지 않도록.clone() 하여 사용한다.

<br />
<br />

### 특정 날짜 이전으로 조정하기

moment.subtract()

```
//2017-01-01 기준 1년 전으로 조정하기

moment("2017-01-01").subtract(1, "year").format()
// 2016-01-01T00:00:00+09:00
```

<br />
<br />

### 요일값 가져오기

moment().day()

```
const handleBirthDayChange = (event) => {
setDay(moment(event.target.value, "YYYY-MM-DD").format("dddd"));
};

// React useState 사용 기준 Day=Thursday (요일값)
```

<br />
<br />

### Moment 결과 한국어로 표기하기

```
// 이전 값 : 07-17-2021

moment("07-17-2021").format("YYYY년 MM월 DD일")
//2021년 07월 17일
```

<br />
<br />

### 날짜 비교하기

moment().diff()

```
moment().diff()

moment("2021-07-17 03:00").diff(moment("2021-07-18 02:00"), "hours")
// -23
```

<br />
<br />

#### React 기준 공부에 사용한 코드

```
import React, { useRef, useState } from "react";
import moment from "moment-timezone";
import "moment/locale/ko"; //국제화 기준 표기법

function MomentExample() {
const birthDayRef = useRef(null);
const [day, setDay] = useState("");
const handleBirthDayChange = (event) => {
setDay(moment(event.target.value, "YYYY-MM-DD").format("dddd"));
};

const momentDate = moment();
const newMomentDate = momentDate.add(1, "week"); // 값이 덮어 씌워진다
const cloneNewMomentDate = newMomentDate.clone().add(1, "week"); // 값이 덮어 씌워지지 않는다
return (
<div>
<h1>Moment</h1>
<div>Immutable Check</div>
<div>
{momentDate.format()}
<br />
{newMomentDate.format()}
<br />
{cloneNewMomentDate.format()}
</div>
<br />

<div>Summer Time (New York)</div>
<div>
2018년 3월 10일 13시에 하루 더하기:
{moment
.tz("2018-03-10 13:00:00", "America/New_York")
.add(1, "day") // 24 hour 로 돌린 결과와 한 시간 차이
.format()}
</div>
<div>
2018년 3월 10일 13시에 24시간 더하기:
{moment
.tz("2018-03-10 13:00:00", "America/New_York")
.add(24, "hour") // 해 시간에 따라서 한 시간이 달라졌기 때문에 1 day 로 실행한 결과와 한 시간 차이
.format()}
</div>
<br />

<div>Leap Year (Korea)</div>
<div>
2017년 1월 1일에 1년 빼기:
{moment("2017-01-01") // 대한민국 윤년 표기
.subtract(1, "year")
.format()}
</div>
<div>
2017년 1월 1일에 365일 빼기:
{moment("2017-01-01").subtract(365, "day").format()}
</div>
<br />
<div>한국어로 표기 07-17-2021을 2021년 7월 17일로 표기</div>
<div>{moment("07-17-2021").format("YYYY년 MM월 DD일")}</div>
<br />

<div>생일 요일 찾기</div>
<div>
<input type="date" ref={birthDayRef} onChange={handleBirthDayChange} />
<div>무슨 요일이었을까?</div>
<div>{day}</div>
</div>
<br />

<div>두 날짜 비교</div>
<div>2021-07-17 03:00 dhk 2021-07-18 02:00는 몇 시간 차이인가?</div>
<div>
{moment("2021-07-17 03:00").diff(moment("2021-07-18 02:00"), "hours")}
시간
</div>
</div>
);
}

export default MomentExample;
```
