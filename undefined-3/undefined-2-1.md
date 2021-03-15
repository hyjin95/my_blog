# 데이터 형태

### 참조형\(reference type\)

* 정수, 실수, 문자, 논리 등의 실제 데이터 값을 저장하는 타입

### 원시형\(premitive type\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC885;</th>
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

### 참조형과 원시형의 차이점

```java
int i = null; //불가능
Integer integer = null; //가능

List<int> i; //불가능
List<Integer> integer //가능
```

1. null을 담을 수 있는지 - 원시형은 null을 담을 수 없다. - 참조형은 null을 담을 수 있다.
2. 제네릭 타입으로 사용할 수 있는지 - 원시형은 제네릭 타입으로 사용할 수 없다. - 참조형은 제네릭 타입으로 사용할 수 있다.

