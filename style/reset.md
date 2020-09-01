## Reset

custom - 2020.08.31

```css
/* @import './font'; 
font-face import
*/

/* all */
* {
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -o-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -webkit-text-size-adjust: none;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
*:focus {
    outline: 0;
}

/* Set body default */
body {
    min-height: 100vh;
    /* font 설정 */
    /* font-family: '영어', '한글', serif;
       font-size: ;
       color: ;
    */
}

/* Inherit fonts */
body * {
    font-size: 100%;
    font: inherit;
    vertical-align: baseline;
}

/* Remove default padding, margin, border */
html,
body,
div,
span,
applet,
object,
iframe,
h1,
h2,
h3,
h4,
h5,
h6,
p,
blockquote,
pre,
a,
abbr,
acronym,
address,
big,
cite,
code,
del,
dfn,
em,
img,
ins,
kbd,
q,
s,
samp,
small,
strike,
strong,
sub,
sup,
tt,
var,
b,
u,
i,
center,
dl,
dt,
dd,
ol,
ul,
li,
fieldset,
form,
label,
legend,
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td,
article,
aside,
canvas,
details,
embed,
figure,
figcaption,
footer,
header,
hgroup,
menu,
nav,
output,
ruby,
section,
summary,
time,
mark,
audio,
video,
textarea,
input {
    padding: 0;
    margin: 0;
    border: 0;
}

/* Semantic tag */
article,
aside,
details,
figcaption,
figure,
footer,
header,
hgroup,
menu,
nav,
section {
    display: block;
}

/* ul, ol, dl tag */
ul,
ol,
dl {
    list-style: none;
}

/* blockquote, q tag */
blockquote,
q {
    quotes: none;
}
blockquote:before,
blockquote:after,
q:before,
q:after {
    content: ”;
    content: none;
}

/* table tag */
table {
    border-collapse: collapse;
    border-spacing: 0;
}
tspan {
    font-size: 10px;
}

/* a tag */
a {
    text-decoration: none;
    color: inherit;
}

/* hr tag */
caption,
hr {
    display: none;
}

/* button tag */
button {
    background: none;
    outline: none;
    cursor: pointer;
}
```

