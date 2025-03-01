---
title: Assertions
slug: Web/JavaScript/Guide/Regular_expressions/Assertions
original_slug: Web/JavaScript/Guide/정규식/Assertions
---

{{jsSidebar("JavaScript Guide")}}

Assertions에는 행이나 단어의 시작 · 끝을 나타내는 경계와 (앞, 뒤 읽고 조건식을 포함한) 어떤 식 으로든 매치가 가능한 것을 나타내는 다른 패턴이 포함됩니다.

{{EmbedInteractiveExample("pages/js/regexp-assertions.html", "taller")}}

## Types

### Boundary-type assertions

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Characters</th>
      <th scope="col">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>^</code></td>
      <td>
        <p>
          Matches the beginning of input. If the multiline flag is set to true,
          also matches immediately after a line break character. For example,
          <code>/^A/</code> does not match the "A" in "an A", but does match the
          first "A" in "An A".
        </p>
        <div class="blockIndicator note">
          <p>
            This character has a different meaning when it appears at the start
            of a
            <a
              href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges"
              >group</a
            >.
          </p>
        </div>
      </td>
    </tr>
    <tr>
      <td><code>$</code></td>
      <td>
        <p>
          Matches the end of input. If the multiline flag is set to true, also
          matches immediately before a line break character. For example,
          <code>/t$/</code> does not match the "t" in "eater", but does match it
          in "eat".
        </p>
      </td>
    </tr>
    <tr>
      <td><code>\b</code></td>
      <td>
        <p>
          Matches a word boundary. This is the position where a word character
          is not followed or preceded by another word-character, such as between
          a letter and a space. Note that a matched word boundary is not
          included in the match. In other words, the length of a matched word
          boundary is zero.
        </p>
        <p>Examples:</p>
        <ul>
          <li><code>/\bm/</code> matches the "m" in "moon".</li>
          <li>
            <code>/oo\b/</code> does not match the "oo" in "moon", because "oo"
            is followed by "n" which is a word character.
          </li>
          <li>
            <code>/oon\b/</code> matches the "oon" in "moon", because "oon" is
            the end of the string, thus not followed by a word character.
          </li>
          <li>
            <code>/\w\b\w/</code> will never match anything, because a word
            character can never be followed by both a non-word and a word
            character.
          </li>
        </ul>
        <p>
          To match a backspace character (<code>[\b]</code>), see
          <a
            href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes"
            >Character Classes</a
          >.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>\B</code></td>
      <td>
        <p>
          Matches a non-word boundary. This is a position where the previous and
          next character are of the same type: Either both must be words, or
          both must be non-words, for example between two letters or between two
          spaces. The beginning and end of a string are considered non-words.
          Same as the matched word boundary, the matched non-word boundary is
          also not included in the match. For example,
          <code>/\Bon/</code> matches "on" in "at noon", and
          <code>/ye\B/</code> matches "ye" in "possibly yesterday".
        </p>
      </td>
    </tr>
  </tbody>
</table>

### Other assertions

<div class="blockIndicator note"><p><strong>Note</strong>: The <code>?</code> character may also be used as a quantifier.</p></div>

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Characters</th>
      <th scope="col">Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>x(?=y)</code></td>
      <td>
        <p>
          <strong>Lookahead assertion: </strong>Matches "x" only if "x" is
          followed by "y". For example, /<code>Jack(?=Sprat)/</code> matches
          "Jack" only if it is followed by "Sprat".<br /><code
            >/Jack(?=Sprat|Frost)/</code
          >
          matches "Jack" only if it is followed by "Sprat" or "Frost". However,
          neither "Sprat" nor "Frost" is part of the match results.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>x(?!y)</code></td>
      <td>
        <p>
          <strong>Negative lookahead assertion: </strong>Matches "x" only if "x"
          is not followed by "y". For example, <code>/\d+(?!\.)/</code> matches
          a number only if it is not followed by a decimal point.
          <code>/\d+(?!\.)/.exec('3.141')</code> matches "141" but not "3.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>(?&#x3C;=y)x</code></td>
      <td>
        <p>
          <strong>Lookbehind assertion: </strong>Matches "x" only if "x" is
          preceded by "y". For example,
          <code>/(?&#x3C;=Jack)Sprat/</code> matches "Sprat" only if it is
          preceded by "Jack". <code>/(?&#x3C;=Jack|Tom)Sprat/</code> matches
          "Sprat" only if it is preceded by "Jack" or "Tom". However, neither
          "Jack" nor "Tom" is part of the match results.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>(?&#x3C;!y)x</code></td>
      <td>
        <p>
          <strong>Negative lookbehind assertion: </strong>Matches "x" only if
          "x" is not preceded by "y". For example,
          <code>/(?&#x3C;!-)\d+/</code> matches a number only if it is not
          preceded by a minus sign.
          <code>/(?&#x3C;!-)\d+/.exec('3')</code> matches "3".
          <code>/(?&#x3C;!-)\d+/.exec('-3')</code> match is not found because
          the number is preceded by the minus sign.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Examples

### General boundary-type overview example

```js
// Using Regex boundaries to fix buggy string.
buggyMultiline = `tey, ihe light-greon apple
tangs on ihe greon traa`;

// 1) Use ^ to fix the matching at the begining of the string, and right after newline.
buggyMultiline = buggyMultiline.replace(/^t/gim,'h');
console.log(1, buggyMultiline); // fix 'tey', 'tangs' => 'hey', 'hangs'. Avoid 'traa'.

// 2) Use $ to fix matching at the end of the text.
buggyMultiline = buggyMultiline.replace(/aa$/gim,'ee.');
console.log(2, buggyMultiline); // fix  'traa' => 'tree'.

// 3) Use \b to match characters right on border between a word and a space.
buggyMultiline = buggyMultiline.replace(/\bi/gim,'t');
console.log(3, buggyMultiline); // fix  'ihe' but does not touch 'light'.

// 4) Use \B to match characters inside borders of an entity.
fixedMultiline = buggyMultiline.replace(/\Bo/gim,'e');
console.log(4, fixedMultiline); // fix  'greon' but does not touch 'on'.
```

### Matching the beginning of an input using a ^ control character

입력 시작시 일치를 위해 `^`를 사용하십시오. 이 예에서는 `/^A/` regex로 'A'로 시작하는 결과를 얻습니다. 여기서 `^`는 한 가지 역할 만합니다. 적절한 결과를 보기위해 [화살표](/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 함수가있는 [필터](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 메소드를 사용합니다.

```js
let fruits = ["Apple", "Watermelon", "Orange", "Avocado", "Strawberry"];

// Select fruits started with 'A' by /^A/ Regex.
// Here '^' control symbol used only in one role: Matching begining of an input.

let fruitsStartsWithA = fruits.filter(fruit => /^A/.test(fruit));
console.log(fruitsStartsWithA); // [ 'Apple', 'Avocado' ]
```

두 번째 예제에서 `^`는 두 가지 모두에 사용됩니다 : 입력의 일치 시작점, [그룹](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges)에서 사용될 때 부정 또는 보완 문자 세트.

```js
let fruits = ["Apple", "Watermelon", "Orange", "Avocado", "Strawberry"];

// Selecting fruits that dose not start by 'A' by a /^[^A]/ regex.
// In this example, two meanings of '^' control symbol are represented:
// 1) Matching begining of the input
// 2) A negated or complemented character set: [^A]
// That is, it matches anything that is not enclosed in the brackets.

let fruitsStartsWithNotA = fruits.filter(fruit => /^[^A]/.test(fruit));

console.log(fruitsStartsWithNotA); // [ 'Watermelon', 'Orange', 'Strawberry' ]
```

### Matching a word boundary

```js
let fruitsWithDescription = ["Red apple", "Orange orange", "Green Avocado"];

// Select descriptions that contains 'en' or 'ed' words endings:
let enEdSelection = fruitsWithDescription.filter(descr => /(en|ed)\b/.test(descr));

console.log(enEdSelection); // [ 'Red apple', 'Green Avocado' ]
```

### Lookahead assertion

```js
// JS Lookahead assertion x(?=y)

let regex = /First(?= test)/g;

console.log('First test'.match(regex)); // [ 'First' ]
console.log('First peach'.match(regex)); // null
console.log('This is a First test in a year.'.match(regex)); // [ 'First' ]
console.log('This is a First peach in a month.'.match(regex)); // null
```

### Basic negative lookahead assertion

For example, `/\d+(?!\.)/` matches a number only if it is not followed by a decimal point. `/\d+(?!\.)/.exec('3.141')` matches "141" but not "3.

```js
console.log(/\d+(?!\.)/g.exec('3.141')); // [ '141', index: 2, input: '3.141' ]
```

### Different meaning of '?!' combination usage in Assertions and Ranges

Different meaning of `?!` combination usage in [Assertions](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions) `/x(?!y)/` and [Ranges](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges) `[^?!]`.

```js
let orangeNotLemon = "Do you want to have an orange? Yes, I do not want to have a lemon!";

// Different meaning of '?!' combination usage in Assertions /x(?!y)/ and  Ranges /[^?!]/
let selectNotLemonRegex = /[^?!]+have(?! a lemon)[^?!]+[?!]/gi
console.log(orangeNotLemon.match(selectNotLemonRegex)); // [ 'Do you want to have an orange?' ]

let selectNotOrangeRegex = /[^?!]+have(?! an orange)[^?!]+[?!]/gi
console.log(orangeNotLemon.match(selectNotOrangeRegex)); // [ ' Yes, I do not want to have a lemon!' ]
```

### Lookbehind assertion

```js
let oranges = ['ripe orange A ', 'green orange B', 'ripe orange C',];

let ripe_oranges = oranges.filter( fruit => fruit.match(/(?<=ripe )orange/));
console.log(ripe_oranges); // [ 'ripe orange A ', 'ripe orange C' ]
```

## 명세서

{{Specifications}}

## Browser compatibility

For browser compatibility information, check out the [main Regular Expressions compatibility table](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#Browser_compatibility).

## See also

- [Regular expressions guide](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

  - [Character classes](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes)
  - [Quantifiers](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Quantifiers)
  - [Unicode property escapes](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Unicode_Property_Escapes)
  - [Groups and ranges](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges)

- [The `RegExp()` constructor](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
