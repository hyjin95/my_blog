# 데이터 형태

## 정리

### 참조형\(reference type\)

* 정수, 실수, 문자, 논리 등의 실제 데이터 값을 저장하는 타입
* 원시형을 제외한 문자열, 배열, 열거, 클래스, 인터페이스 등을 말한다.
* stack 메모리에는 주소 값이, 실제 값은 heap 메모리에 존재한다. 객체 사용시마다 참조변수에 저장된 객체의 주소를 불러와 사용하는 방식이다.

### 원시형\(premitive type\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC885;&#xB958;</th>
      <th style="text-align:left">&#xB370;&#xC774;&#xD130;&#xD615;</th>
      <th style="text-align:left">&#xD06C;&#xAE30;(byte/bit)</th>
      <th style="text-align:left">&#xD45C;&#xD604;&#xBC94;&#xC704;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#xB17C;&#xB9AC;&#xD615;</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">1 / 8</td>
      <td style="text-align:left">true &#xB610;&#xB294; false</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xBB38;&#xC790;&#xD615;</td>
      <td style="text-align:left">char</td>
      <td style="text-align:left">2 / 16</td>
      <td style="text-align:left">
        <p>&apos;\u0000&apos; ~ &apos;uFFFF&apos;</p>
        <p>(16&#xBE44;&#xD2B8; &#xC720;&#xB2C8;&#xCF54;&#xB4DC; &#xBB38;&#xC790;
          &#xB370;&#xC774;&#xD130;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC815;&#xC218;&#xD615;</td>
      <td style="text-align:left">byte</td>
      <td style="text-align:left">1 / 8</td>
      <td style="text-align:left">-128 ~ 127</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC815;&#xC218;&#xD615;</td>
      <td style="text-align:left">short</td>
      <td style="text-align:left">2 / 16</td>
      <td style="text-align:left">-32768 ~ 32767</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC815;&#xC218;&#xD615;</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">4 / 32</td>
      <td style="text-align:left">
        <p>-2147483648 ~ 2147483647</p>
        <p>( -21&#xC5B5; ~ + 21&#xC5B5;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC815;&#xC218;&#xD615;</td>
      <td style="text-align:left">long</td>
      <td style="text-align:left">8 / 64</td>
      <td style="text-align:left">
        <p>-9223372036854775808 ~ 9223372036854775807</p>
        <p>(-100&#xACBD; ~ + 100&#xACBD;)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC2E4;&#xC218;&#xD615;</td>
      <td style="text-align:left">float</td>
      <td style="text-align:left">4 / 32</td>
      <td style="text-align:left">1.4E-45 ~ 3.4028235E38</td>
    </tr>
    <tr>
      <td style="text-align:left">&#xC2E4;&#xC218;&#xD615;</td>
      <td style="text-align:left">double</td>
      <td style="text-align:left">8 / 64</td>
      <td style="text-align:left">4.9E-324 ~ 1.7976931348623157E308</td>
    </tr>
  </tbody>
</table>

* 객체\(Object\)의 주소를 저장하는 타입 메모리 주소 값을 통해 객체를 참조하는 타입
* stack 메모리에 값이 존재한다.

### 참조형과 원시형의 차이점

```java
int i = null; //불가능
Integer integer = null; //가능

List<int> i; //불가능
List<Integer> integer //가능
```

1. null을 담을 수 있는지 - 원시형은 null을 담을 수 없다. - 참조형은 null을 담을 수 있다.
2. 제네릭 타입으로 사용할 수 있는지 - 원시형은 제네릭 타입으로 사용할 수 없다. - 참조형은 제네릭 타입으로 사용할 수 있다.

### 원시형의 장점

![&#xD0C0;&#xC785;&#xBCC4; &#xD3C9;&#xADE0; &#xC751;&#xB2F5;&#xC2DC;&#xAC04;](../.gitbook/assets/1.gif)

1. 제네릭 타입으로도, null값을 가질 수 도 없는 원시형의 장점은 '성능\(접근속도\)'에 있다. - 참조형은 값을 불러올때마다 언박싱 과정을 거쳐 원시형에 비해 접근 속도가 느리다. - 단, 매우 큰 값을 복사하는 등 의 경우에는 참조값만 넘길 수 있는 참조형이 나을때가 있다.
2. 차지하는 메모리 양 - 참조형이 차이하는 메모리 크기가 더 크다. - boolean 1bit &lt; Boolean 128 bits

### 사용시기

* 속도와 메모리 효율에 있어 우수한 원시형을 먼저 고려해보고, null을 다뤄야 하거나 제네릭타입으로 사용해야 하는 경우에는 참조형을 사용한다.

## 더보기

