---
title: Test-Time Distribution Normalization For Contrastively Learned Vision-language Models
layout: default
---

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml-full.js" type="text/javascript"></script>

<style>
    td {
        text-align: center;
        font-size: 11pt;
    }

    a:hover {
        text-decoration: underline;
    }
</style>

<style id="MJX-CHTML-styles">
mjx-container[jax="CHTML"] {
  line-height: 0;
}

mjx-container [space="1"] {
  margin-left: .111em;
}

mjx-container [space="2"] {
  margin-left: .167em;
}

mjx-container [space="3"] {
  margin-left: .222em;
}

mjx-container [space="4"] {
  margin-left: .278em;
}

mjx-container [space="5"] {
  margin-left: .333em;
}

mjx-container [rspace="1"] {
  margin-right: .111em;
}

mjx-container [rspace="2"] {
  margin-right: .167em;
}

mjx-container [rspace="3"] {
  margin-right: .222em;
}

mjx-container [rspace="4"] {
  margin-right: .278em;
}

mjx-container [rspace="5"] {
  margin-right: .333em;
}

mjx-container [size="s"] {
  font-size: 70.7%;
}

mjx-container [size="ss"] {
  font-size: 50%;
}

mjx-container [size="Tn"] {
  font-size: 60%;
}

mjx-container [size="sm"] {
  font-size: 85%;
}

mjx-container [size="lg"] {
  font-size: 120%;
}

mjx-container [size="Lg"] {
  font-size: 144%;
}

mjx-container [size="LG"] {
  font-size: 173%;
}

mjx-container [size="hg"] {
  font-size: 207%;
}

mjx-container [size="HG"] {
  font-size: 249%;
}

mjx-container [width="full"] {
  width: 100%;
}

mjx-box {
  display: inline-block;
}

mjx-block {
  display: block;
}

mjx-itable {
  display: inline-table;
}

mjx-row {
  display: table-row;
}

mjx-row > * {
  display: table-cell;
}

mjx-mtext {
  display: inline-block;
  text-align: left;
}

mjx-mstyle {
  display: inline-block;
}

mjx-merror {
  display: inline-block;
  color: red;
  background-color: yellow;
}

mjx-mphantom {
  visibility: hidden;
}

_::-webkit-full-page-media, _:future, :root mjx-container {
  will-change: opacity;
}

mjx-assistive-mml {
  position: absolute !important;
  top: 0px;
  left: 0px;
  clip: rect(1px, 1px, 1px, 1px);
  padding: 1px 0px 0px 0px !important;
  border: 0px !important;
  display: block !important;
  width: auto !important;
  overflow: hidden !important;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

mjx-assistive-mml[display="block"] {
  width: 100% !important;
}

mjx-math {
  display: inline-block;
  text-align: left;
  line-height: 0;
  text-indent: 0;
  font-style: normal;
  font-weight: normal;
  font-size: 100%;
  font-size-adjust: none;
  letter-spacing: normal;
  border-collapse: collapse;
  word-wrap: normal;
  word-spacing: normal;
  white-space: nowrap;
  direction: ltr;
  padding: 1px 0;
}

mjx-container[jax="CHTML"][display="true"] {
  display: block;
  text-align: center;
  margin: 1em 0;
}

mjx-container[jax="CHTML"][display="true"][width="full"] {
  display: flex;
}

mjx-container[jax="CHTML"][display="true"] mjx-math {
  padding: 0;
}

mjx-container[jax="CHTML"][justify="left"] {
  text-align: left;
}

mjx-container[jax="CHTML"][justify="right"] {
  text-align: right;
}

mjx-munder {
  display: inline-block;
  text-align: left;
}

mjx-over {
  text-align: left;
}

mjx-munder:not([limits="false"]) {
  display: inline-table;
}

mjx-munder > mjx-row {
  text-align: left;
}

mjx-under {
  padding-bottom: .1em;
}

mjx-TeXAtom {
  display: inline-block;
  text-align: left;
}

mjx-mrow {
  display: inline-block;
  text-align: left;
}

mjx-mi {
  display: inline-block;
  text-align: left;
}

mjx-c {
  display: inline-block;
}

mjx-utext {
  display: inline-block;
  padding: .75em 0 .2em 0;
}

mjx-mo {
  display: inline-block;
  text-align: left;
}

mjx-stretchy-h {
  display: inline-table;
  width: 100%;
}

mjx-stretchy-h > * {
  display: table-cell;
  width: 0;
}

mjx-stretchy-h > * > mjx-c {
  display: inline-block;
  transform: scalex(1.0000001);
}

mjx-stretchy-h > * > mjx-c::before {
  display: inline-block;
  width: initial;
}

mjx-stretchy-h > mjx-ext {
  /* IE */ overflow: hidden;
  /* others */ overflow: clip visible;
  width: 100%;
}

mjx-stretchy-h > mjx-ext > mjx-c::before {
  transform: scalex(500);
}

mjx-stretchy-h > mjx-ext > mjx-c {
  width: 0;
}

mjx-stretchy-h > mjx-beg > mjx-c {
  margin-right: -.1em;
}

mjx-stretchy-h > mjx-end > mjx-c {
  margin-left: -.1em;
}

mjx-stretchy-v {
  display: inline-block;
}

mjx-stretchy-v > * {
  display: block;
}

mjx-stretchy-v > mjx-beg {
  height: 0;
}

mjx-stretchy-v > mjx-end > mjx-c {
  display: block;
}

mjx-stretchy-v > * > mjx-c {
  transform: scaley(1.0000001);
  transform-origin: left center;
  overflow: hidden;
}

mjx-stretchy-v > mjx-ext {
  display: block;
  height: 100%;
  box-sizing: border-box;
  border: 0px solid transparent;
  /* IE */ overflow: hidden;
  /* others */ overflow: visible clip;
}

mjx-stretchy-v > mjx-ext > mjx-c::before {
  width: initial;
  box-sizing: border-box;
}

mjx-stretchy-v > mjx-ext > mjx-c {
  transform: scaleY(500) translateY(.075em);
  overflow: visible;
}

mjx-mark {
  display: inline-block;
  height: 0px;
}

mjx-msub {
  display: inline-block;
  text-align: left;
}

mjx-msup {
  display: inline-block;
  text-align: left;
}

mjx-mn {
  display: inline-block;
  text-align: left;
}

mjx-mover {
  display: inline-block;
  text-align: left;
}

mjx-mover:not([limits="false"]) {
  padding-top: .1em;
}

mjx-mover:not([limits="false"]) > * {
  display: block;
  text-align: left;
}

mjx-mfrac {
  display: inline-block;
  text-align: left;
}

mjx-frac {
  display: inline-block;
  vertical-align: 0.17em;
  padding: 0 .22em;
}

mjx-frac[type="d"] {
  vertical-align: .04em;
}

mjx-frac[delims] {
  padding: 0 .1em;
}

mjx-frac[atop] {
  padding: 0 .12em;
}

mjx-frac[atop][delims] {
  padding: 0;
}

mjx-dtable {
  display: inline-table;
  width: 100%;
}

mjx-dtable > * {
  font-size: 2000%;
}

mjx-dbox {
  display: block;
  font-size: 5%;
}

mjx-num {
  display: block;
  text-align: center;
}

mjx-den {
  display: block;
  text-align: center;
}

mjx-mfrac[bevelled] > mjx-num {
  display: inline-block;
}

mjx-mfrac[bevelled] > mjx-den {
  display: inline-block;
}

mjx-den[align="right"], mjx-num[align="right"] {
  text-align: right;
}

mjx-den[align="left"], mjx-num[align="left"] {
  text-align: left;
}

mjx-nstrut {
  display: inline-block;
  height: .054em;
  width: 0;
  vertical-align: -.054em;
}

mjx-nstrut[type="d"] {
  height: .217em;
  vertical-align: -.217em;
}

mjx-dstrut {
  display: inline-block;
  height: .505em;
  width: 0;
}

mjx-dstrut[type="d"] {
  height: .726em;
}

mjx-line {
  display: block;
  box-sizing: border-box;
  min-height: 1px;
  height: .06em;
  border-top: .06em solid;
  margin: .06em -.1em;
  overflow: hidden;
}

mjx-line[type="d"] {
  margin: .18em -.1em;
}

mjx-munderover {
  display: inline-block;
  text-align: left;
}

mjx-munderover:not([limits="false"]) {
  padding-top: .1em;
}

mjx-munderover:not([limits="false"]) > * {
  display: block;
}

mjx-msubsup {
  display: inline-block;
  text-align: left;
}

mjx-script {
  display: inline-block;
  padding-right: .05em;
  padding-left: .033em;
}

mjx-script > mjx-spacer {
  display: block;
}

mjx-mspace {
  display: inline-block;
  text-align: left;
}

mjx-mtable {
  display: inline-block;
  text-align: center;
  vertical-align: .25em;
  position: relative;
  box-sizing: border-box;
  border-spacing: 0;
  border-collapse: collapse;
}

mjx-mstyle[size="s"] mjx-mtable {
  vertical-align: .354em;
}

mjx-labels {
  position: absolute;
  left: 0;
  top: 0;
}

mjx-table {
  display: inline-block;
  vertical-align: -.5ex;
  box-sizing: border-box;
}

mjx-table > mjx-itable {
  vertical-align: middle;
  text-align: left;
  box-sizing: border-box;
}

mjx-labels > mjx-itable {
  position: absolute;
  top: 0;
}

mjx-mtable[justify="left"] {
  text-align: left;
}

mjx-mtable[justify="right"] {
  text-align: right;
}

mjx-mtable[justify="left"][side="left"] {
  padding-right: 0 ! important;
}

mjx-mtable[justify="left"][side="right"] {
  padding-left: 0 ! important;
}

mjx-mtable[justify="right"][side="left"] {
  padding-right: 0 ! important;
}

mjx-mtable[justify="right"][side="right"] {
  padding-left: 0 ! important;
}

mjx-mtable[align] {
  vertical-align: baseline;
}

mjx-mtable[align="top"] > mjx-table {
  vertical-align: top;
}

mjx-mtable[align="bottom"] > mjx-table {
  vertical-align: bottom;
}

mjx-mtable[side="right"] mjx-labels {
  min-width: 100%;
}

mjx-mtr {
  display: table-row;
  text-align: left;
}

mjx-mtr[rowalign="top"] > mjx-mtd {
  vertical-align: top;
}

mjx-mtr[rowalign="center"] > mjx-mtd {
  vertical-align: middle;
}

mjx-mtr[rowalign="bottom"] > mjx-mtd {
  vertical-align: bottom;
}

mjx-mtr[rowalign="baseline"] > mjx-mtd {
  vertical-align: baseline;
}

mjx-mtr[rowalign="axis"] > mjx-mtd {
  vertical-align: .25em;
}

mjx-mtd {
  display: table-cell;
  text-align: center;
  padding: .215em .4em;
}

mjx-mtd:first-child {
  padding-left: 0;
}

mjx-mtd:last-child {
  padding-right: 0;
}

mjx-mtable > * > mjx-itable > *:first-child > mjx-mtd {
  padding-top: 0;
}

mjx-mtable > * > mjx-itable > *:last-child > mjx-mtd {
  padding-bottom: 0;
}

mjx-tstrut {
  display: inline-block;
  height: 1em;
  vertical-align: -.25em;
}

mjx-labels[align="left"] > mjx-mtr > mjx-mtd {
  text-align: left;
}

mjx-labels[align="right"] > mjx-mtr > mjx-mtd {
  text-align: right;
}

mjx-mtd[extra] {
  padding: 0;
}

mjx-mtd[rowalign="top"] {
  vertical-align: top;
}

mjx-mtd[rowalign="center"] {
  vertical-align: middle;
}

mjx-mtd[rowalign="bottom"] {
  vertical-align: bottom;
}

mjx-mtd[rowalign="baseline"] {
  vertical-align: baseline;
}

mjx-mtd[rowalign="axis"] {
  vertical-align: .25em;
}

mjx-msqrt {
  display: inline-block;
  text-align: left;
}

mjx-root {
  display: inline-block;
  white-space: nowrap;
}

mjx-surd {
  display: inline-block;
  vertical-align: top;
}

mjx-sqrt {
  display: inline-block;
  padding-top: .07em;
}

mjx-sqrt > mjx-box {
  border-top: .07em solid;
}

mjx-sqrt.mjx-tall > mjx-box {
  padding-left: .3em;
  margin-left: -.3em;
}

mjx-c::before {
  display: block;
  width: 0;
}

.MJX-TEX {
  font-family: MJXZERO, MJXTEX;
}

.TEX-B {
  font-family: MJXZERO, MJXTEX-B;
}

.TEX-I {
  font-family: MJXZERO, MJXTEX-I;
}

.TEX-MI {
  font-family: MJXZERO, MJXTEX-MI;
}

.TEX-BI {
  font-family: MJXZERO, MJXTEX-BI;
}

.TEX-S1 {
  font-family: MJXZERO, MJXTEX-S1;
}

.TEX-S2 {
  font-family: MJXZERO, MJXTEX-S2;
}

.TEX-S3 {
  font-family: MJXZERO, MJXTEX-S3;
}

.TEX-S4 {
  font-family: MJXZERO, MJXTEX-S4;
}

.TEX-A {
  font-family: MJXZERO, MJXTEX-A;
}

.TEX-C {
  font-family: MJXZERO, MJXTEX-C;
}

.TEX-CB {
  font-family: MJXZERO, MJXTEX-CB;
}

.TEX-FR {
  font-family: MJXZERO, MJXTEX-FR;
}

.TEX-FRB {
  font-family: MJXZERO, MJXTEX-FRB;
}

.TEX-SS {
  font-family: MJXZERO, MJXTEX-SS;
}

.TEX-SSB {
  font-family: MJXZERO, MJXTEX-SSB;
}

.TEX-SSI {
  font-family: MJXZERO, MJXTEX-SSI;
}

.TEX-SC {
  font-family: MJXZERO, MJXTEX-SC;
}

.TEX-T {
  font-family: MJXZERO, MJXTEX-T;
}

.TEX-V {
  font-family: MJXZERO, MJXTEX-V;
}

.TEX-VB {
  font-family: MJXZERO, MJXTEX-VB;
}

mjx-stretchy-v mjx-c, mjx-stretchy-h mjx-c {
  font-family: MJXZERO, MJXTEX-S1, MJXTEX-S4, MJXTEX, MJXTEX-A ! important;
}

@font-face /* 0 */ {
  font-family: MJXZERO;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Zero.woff") format("woff");
}

@font-face /* 1 */ {
  font-family: MJXTEX;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Regular.woff") format("woff");
}

@font-face /* 2 */ {
  font-family: MJXTEX-B;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Bold.woff") format("woff");
}

@font-face /* 3 */ {
  font-family: MJXTEX-I;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Math-Italic.woff") format("woff");
}

@font-face /* 4 */ {
  font-family: MJXTEX-MI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Main-Italic.woff") format("woff");
}

@font-face /* 5 */ {
  font-family: MJXTEX-BI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Math-BoldItalic.woff") format("woff");
}

@font-face /* 6 */ {
  font-family: MJXTEX-S1;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size1-Regular.woff") format("woff");
}

@font-face /* 7 */ {
  font-family: MJXTEX-S2;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size2-Regular.woff") format("woff");
}

@font-face /* 8 */ {
  font-family: MJXTEX-S3;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size3-Regular.woff") format("woff");
}

@font-face /* 9 */ {
  font-family: MJXTEX-S4;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Size4-Regular.woff") format("woff");
}

@font-face /* 10 */ {
  font-family: MJXTEX-A;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_AMS-Regular.woff") format("woff");
}

@font-face /* 11 */ {
  font-family: MJXTEX-C;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Calligraphic-Regular.woff") format("woff");
}

@font-face /* 12 */ {
  font-family: MJXTEX-CB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Calligraphic-Bold.woff") format("woff");
}

@font-face /* 13 */ {
  font-family: MJXTEX-FR;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Fraktur-Regular.woff") format("woff");
}

@font-face /* 14 */ {
  font-family: MJXTEX-FRB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Fraktur-Bold.woff") format("woff");
}

@font-face /* 15 */ {
  font-family: MJXTEX-SS;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Regular.woff") format("woff");
}

@font-face /* 16 */ {
  font-family: MJXTEX-SSB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Bold.woff") format("woff");
}

@font-face /* 17 */ {
  font-family: MJXTEX-SSI;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_SansSerif-Italic.woff") format("woff");
}

@font-face /* 18 */ {
  font-family: MJXTEX-SC;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Script-Regular.woff") format("woff");
}

@font-face /* 19 */ {
  font-family: MJXTEX-T;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Typewriter-Regular.woff") format("woff");
}

@font-face /* 20 */ {
  font-family: MJXTEX-V;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Vector-Regular.woff") format("woff");
}

@font-face /* 21 */ {
  font-family: MJXTEX-VB;
  src: url("https://cdn.jsdelivr.net/npm/mathjax@3/es5/output/chtml/fonts/woff-v2/MathJax_Vector-Bold.woff") format("woff");
}

mjx-stretchy-h.mjx-c23DF mjx-beg mjx-c::before {
  content: "\E152";
  padding: 0.32em 0 0.2em 0;
}

mjx-stretchy-h.mjx-c23DF mjx-ext mjx-c::before {
  content: "\E154";
  padding: 0.32em 0 0.2em 0;
}

mjx-stretchy-h.mjx-c23DF mjx-end mjx-c::before {
  content: "\E153";
  padding: 0.32em 0 0.2em 0;
}

mjx-stretchy-h.mjx-c23DF mjx-mid mjx-c::before {
  content: "\E151\E150";
  padding: 0.32em 0 0.2em 0;
}

mjx-stretchy-h.mjx-c23DF > mjx-ext {
  width: 50%;
}

.mjx-stretched mjx-c.mjx-c2013::before {
  content: "\2013";
}

mjx-stretchy-h.mjx-c2013 mjx-ext mjx-c::before {
  content: "\2013";
  padding: 0.285em 0 0 0;
}

mjx-c.mjx-c1D53C.TEX-A::before {
  padding: 0.683em 0.667em 0 0;
  content: "E";
}

mjx-c.mjx-c5B::before {
  padding: 0.75em 0.278em 0.25em 0;
  content: "[";
}

mjx-c.mjx-c28::before {
  padding: 0.75em 0.389em 0.25em 0;
  content: "(";
}

mjx-c.mjx-c210E.TEX-I::before {
  padding: 0.694em 0.576em 0.011em 0;
  content: "h";
}

mjx-c.mjx-c1D437.TEX-I::before {
  padding: 0.683em 0.828em 0 0;
  content: "D";
}

mjx-c.mjx-c1D465.TEX-I::before {
  padding: 0.442em 0.572em 0.011em 0;
  content: "x";
}

mjx-c.mjx-c29::before {
  padding: 0.75em 0.389em 0.25em 0;
  content: ")";
}

mjx-c.mjx-c2212::before {
  padding: 0.583em 0.778em 0.082em 0;
  content: "\2212";
}

mjx-c.mjx-c1D466.TEX-I::before {
  padding: 0.442em 0.49em 0.205em 0;
  content: "y";
}

mjx-c.mjx-c32::before {
  padding: 0.666em 0.5em 0 0;
  content: "2";
}

mjx-c.mjx-c5D::before {
  padding: 0.75em 0.278em 0.25em 0;
  content: "]";
}

mjx-c.mjx-c45::before {
  padding: 0.68em 0.681em 0 0;
  content: "E";
}

mjx-c.mjx-c72::before {
  padding: 0.442em 0.392em 0 0;
  content: "r";
}

mjx-c.mjx-c6F::before {
  padding: 0.448em 0.5em 0.01em 0;
  content: "o";
}

mjx-c.mjx-c3D::before {
  padding: 0.583em 0.778em 0.082em 0;
  content: "=";
}

mjx-c.mjx-cAF::before {
  padding: 0.59em 0.5em 0 0;
  content: "\AF";
}

mjx-c.mjx-c56::before {
  padding: 0.683em 0.75em 0.022em 0;
  content: "V";
}

mjx-c.mjx-c61::before {
  padding: 0.448em 0.5em 0.011em 0;
  content: "a";
}

mjx-c.mjx-c69::before {
  padding: 0.669em 0.278em 0 0;
  content: "i";
}

mjx-c.mjx-c6E::before {
  padding: 0.442em 0.556em 0 0;
  content: "n";
}

mjx-c.mjx-c63::before {
  padding: 0.448em 0.444em 0.011em 0;
  content: "c";
}

mjx-c.mjx-c65::before {
  padding: 0.448em 0.444em 0.011em 0;
  content: "e";
}

mjx-c.mjx-c2B::before {
  padding: 0.583em 0.778em 0.082em 0;
  content: "+";
}

mjx-c.mjx-c42::before {
  padding: 0.683em 0.708em 0 0;
  content: "B";
}

mjx-c.mjx-c73::before {
  padding: 0.448em 0.394em 0.011em 0;
  content: "s";
}

mjx-c.mjx-c4E::before {
  padding: 0.683em 0.75em 0 0;
  content: "N";
}

mjx-c.mjx-c2192::before {
  padding: 0.511em 1em 0.011em 0;
  content: "\2192";
}

mjx-c.mjx-c1D456.TEX-I::before {
  padding: 0.661em 0.345em 0.011em 0;
  content: "i";
}

mjx-c.mjx-c31::before {
  padding: 0.666em 0.5em 0 0;
  content: "1";
}

mjx-c.mjx-c1D45A.TEX-I::before {
  padding: 0.442em 0.878em 0.011em 0;
  content: "m";
}

mjx-c.mjx-c2211.TEX-S2::before {
  padding: 0.95em 1.444em 0.45em 0;
  content: "\2211";
}

mjx-c.mjx-cA0::before {
  padding: 0 0.25em 0 0;
  content: "\A0";
}

mjx-c.mjx-c221E::before {
  padding: 0.442em 1em 0.011em 0;
  content: "\221E";
}

mjx-c.mjx-c2C::before {
  padding: 0.121em 0.278em 0.194em 0;
  content: ",";
}

mjx-c.mjx-c2E::before {
  padding: 0.12em 0.278em 0 0;
  content: ".";
}

mjx-c.mjx-c1D45B.TEX-I::before {
  padding: 0.442em 0.6em 0.011em 0;
  content: "n";
}

mjx-c.mjx-c1D443.TEX-I::before {
  padding: 0.683em 0.751em 0 0;
  content: "P";
}

mjx-c.mjx-c5E::before {
  padding: 0.694em 0.5em 0 0;
  content: "^";
}

mjx-c.mjx-c1D44E.TEX-I::before {
  padding: 0.441em 0.529em 0.01em 0;
  content: "a";
}

mjx-c.mjx-c1D460.TEX-I::before {
  padding: 0.442em 0.469em 0.01em 0;
  content: "s";
}

mjx-c.mjx-c30::before {
  padding: 0.666em 0.5em 0.022em 0;
  content: "0";
}

mjx-c.mjx-c1D444.TEX-I::before {
  padding: 0.704em 0.791em 0.194em 0;
  content: "Q";
}

mjx-c.mjx-c1D44B.TEX-I::before {
  padding: 0.683em 0.852em 0 0;
  content: "X";
}

mjx-c.mjx-c1D44C.TEX-I::before {
  padding: 0.683em 0.763em 0 0;
  content: "Y";
}

mjx-c.mjx-c7C::before {
  padding: 0.75em 0.278em 0.249em 0;
  content: "|";
}

mjx-c.mjx-c1D431.TEX-B::before {
  padding: 0.444em 0.607em 0 0;
  content: "x";
}

mjx-c.mjx-c1D422.TEX-B::before {
  padding: 0.695em 0.319em 0 0;
  content: "i";
}

mjx-c.mjx-c2200::before {
  padding: 0.694em 0.556em 0.022em 0;
  content: "\2200";
}

mjx-c.mjx-c2208::before {
  padding: 0.54em 0.667em 0.04em 0;
  content: "\2208";
}

mjx-c.mjx-c223C::before {
  padding: 0.367em 0.778em 0 0;
  content: "\223C";
}

mjx-c.mjx-c2229::before {
  padding: 0.598em 0.667em 0.022em 0;
  content: "\2229";
}

mjx-c.mjx-c2211.TEX-S1::before {
  padding: 0.75em 1.056em 0.25em 0;
  content: "\2211";
}

mjx-c.mjx-c219B.TEX-A::before {
  padding: 0.437em 1em 0 0;
  content: "\219B";
}

mjx-c.mjx-c1D45D.TEX-I::before {
  padding: 0.442em 0.503em 0.194em 0;
  content: "p";
}

mjx-c.mjx-c3A9::before {
  padding: 0.704em 0.722em 0 0;
  content: "\3A9";
}

mjx-c.mjx-c1D441.TEX-I::before {
  padding: 0.683em 0.888em 0 0;
  content: "N";
}

mjx-c.mjx-c1D458.TEX-I::before {
  padding: 0.694em 0.521em 0.011em 0;
  content: "k";
}

mjx-c.mjx-c28.TEX-S3::before {
  padding: 1.45em 0.736em 0.949em 0;
  content: "(";
}

mjx-c.mjx-c29.TEX-S3::before {
  padding: 1.45em 0.736em 0.949em 0;
  content: ")";
}

mjx-c.mjx-c50::before {
  padding: 0.683em 0.681em 0 0;
  content: "P";
}

mjx-c.mjx-c62::before {
  padding: 0.694em 0.556em 0.011em 0;
  content: "b";
}

mjx-c.mjx-c6C::before {
  padding: 0.694em 0.278em 0 0;
  content: "l";
}

mjx-c.mjx-c74::before {
  padding: 0.615em 0.389em 0.01em 0;
  content: "t";
}

mjx-c.mjx-c79::before {
  padding: 0.431em 0.528em 0.204em 0;
  content: "y";
}

mjx-c.mjx-c20::before {
  padding: 0 0.25em 0 0;
  content: " ";
}

mjx-c.mjx-c68::before {
  padding: 0.694em 0.556em 0 0;
  content: "h";
}

mjx-c.mjx-c6B::before {
  padding: 0.694em 0.528em 0 0;
  content: "k";
}

mjx-c.mjx-c70::before {
  padding: 0.442em 0.556em 0.194em 0;
  content: "p";
}

mjx-c.mjx-c66::before {
  padding: 0.705em 0.372em 0 0;
  content: "f";
}

mjx-c.mjx-c44::before {
  padding: 0.683em 0.764em 0 0;
  content: "D";
}

mjx-c.mjx-c78::before {
  padding: 0.431em 0.528em 0 0;
  content: "x";
}

mjx-c.mjx-c64::before {
  padding: 0.694em 0.556em 0.011em 0;
  content: "d";
}

mjx-c.mjx-c76::before {
  padding: 0.431em 0.528em 0.011em 0;
  content: "v";
}

mjx-c.mjx-c75::before {
  padding: 0.442em 0.556em 0.011em 0;
  content: "u";
}

mjx-c.mjx-c6D::before {
  padding: 0.442em 0.833em 0 0;
  content: "m";
}

mjx-c.mjx-c77::before {
  padding: 0.431em 0.722em 0.011em 0;
  content: "w";
}

mjx-c.mjx-c1D539.TEX-A::before {
  padding: 0.683em 0.667em 0 0;
  content: "B";
}

mjx-c.mjx-c2190::before {
  padding: 0.511em 1em 0.011em 0;
  content: "\2190";
}

mjx-c.mjx-c1D447.TEX-I::before {
  padding: 0.677em 0.704em 0 0;
  content: "T";
}

mjx-c.mjx-c1D434.TEX-I::before {
  padding: 0.716em 0.75em 0 0;
  content: "A";
}

mjx-c.mjx-c21::before {
  padding: 0.716em 0.278em 0 0;
  content: "!";
}

mjx-c.mjx-c2032::before {
  padding: 0.56em 0.275em 0 0;
  content: "\2032";
}

mjx-c.mjx-c1D459.TEX-I::before {
  padding: 0.694em 0.298em 0.011em 0;
  content: "l";
}

mjx-c.mjx-c1D461.TEX-I::before {
  padding: 0.626em 0.361em 0.011em 0;
  content: "t";
}

mjx-c.mjx-c2026::before {
  padding: 0.12em 1.172em 0 0;
  content: "\2026";
}

mjx-c.mjx-c1D457.TEX-I::before {
  padding: 0.661em 0.412em 0.204em 0;
  content: "j";
}

mjx-c.mjx-c1D446.TEX-I::before {
  padding: 0.705em 0.645em 0.022em 0;
  content: "S";
}

mjx-c.mjx-c7B::before {
  padding: 0.75em 0.5em 0.25em 0;
  content: "{";
}

mjx-c.mjx-c2209::before {
  padding: 0.716em 0.667em 0.215em 0;
  content: "\2209";
}

mjx-c.mjx-c7D::before {
  padding: 0.75em 0.5em 0.25em 0;
  content: "}";
}

mjx-c.mjx-c7E::before {
  padding: 0.318em 0.5em 0 0;
  content: "~";
}

mjx-c.mjx-c1D716.TEX-I::before {
  padding: 0.431em 0.406em 0.011em 0;
  content: "\3F5";
}

mjx-c.mjx-c4F::before {
  padding: 0.705em 0.778em 0.022em 0;
  content: "O";
}

mjx-c.mjx-c2264::before {
  padding: 0.636em 0.778em 0.138em 0;
  content: "\2264";
}

mjx-c.mjx-c1D451.TEX-I::before {
  padding: 0.694em 0.52em 0.01em 0;
  content: "d";
}

mjx-c.mjx-c221A::before {
  padding: 0.8em 0.853em 0.2em 0;
  content: "\221A";
}

mjx-c.mjx-c222A::before {
  padding: 0.598em 0.667em 0.022em 0;
  content: "\222A";
}

mjx-c.mjx-c1D435.TEX-I::before {
  padding: 0.683em 0.759em 0 0;
  content: "B";
}

mjx-c.mjx-c2205::before {
  padding: 0.772em 0.5em 0.078em 0;
  content: "\2205";
}
</style>

### TL;DR

We present **Distribution Normalization**, a test-time plug-in module that enhances CLIP's performance on a wide range of zero-shot tasks with less than 5 lines of additional code.

<p align="center">
    <img src="images/dn.gif" alt="Image" />
    <figcaption>An illustration of how Distribution Normalization better aligns with infoNCE pre-training objective. $\mu_{img}$ and $\mu_{txt}$ are mean encoded representations for images and text respectively.</figcaption>
</p>

### Abstract

Advances in the field of vision-language contrastive learning have made it possible for many downstream applications to be carried out efficiently and accurately by simply taking the dot product between image and text representations.

One of the most representative approaches proposed recently known as [CLIP](https://arxiv.org/abs/2103.00020) has garnered widespread adoption due to its effectiveness. CLIP is trained with an InfoNCE loss that takes into account both positive and negative samples to help learn a much more robust representation space. This paper reveals that the common downstream practice of taking a dot product is only a zeroth-order approximation of the optimization goal, resulting in a loss of information during test-time. Intuitively, since the model has been optimized based on the InfoNCE loss, test-time procedures should also be in alignment. The question lies in how one can retrieve any semblance of negative samples information during inference in a computationally efficient way.

To this end, we propose Distribution Normalization (DN), where we approximate the mean representation of a batch of test samples and use such a mean to represent what would be analogous to negative samples in the InfoNCE loss. DN requires no retraining or fine-tuning and can be effortlessly applied during inference. Extensive experiments on a wide variety of downstream tasks exhibit a clear advantage of DN over the dot product on top of other existing test-time augmentation methods.

### Motivation

The InfoNCE loss considers negative samples from the data distribution per positive sample and has been shown to be very effective for cross-modal representation learning. In contrast, during test-time, the similarity is often measured using the dot product between the image and the text representations, which does not utilize any information with regard to the data distribution.

### Methodology

Our paper aims to rectify such misalignment, and we show that this boosts performance consistently across a variety of downstream tasks. However, naively applying InfoNCE loss for downstream tasks requires iterating over all negative samples for every test sample, which is not a tractable operation. Instead, we found that a first-order approximation of the InfoNCE loss, consisting of simply subtracting the distribution mean in the representation space before taking the dot product, is able to achieve a similar effect. We call this proposed approach Distribution Normalization (DN) &mdash; DN is very easy to implement and does not require any retraining, fine-tuning or labeled data.

While a more thorough proof is provided in our paper, we will briefly touch on the core idea here. In our analysis, we let $\mathcal{D}_S$ represent the training distribution and $\mathcal{D}_T$ represent the test distribution, $x_0, y_0$ represent an image and text pair, $\phi$ and $\psi$ represent image and text encoders respectively, and $\tau$ represent a temperature hyperparameter used in InfoNCE loss while training. Then, we observe a zero-order approximation of the InfoNCE loss can be expressed as

$$
    \mathcal{L}_{NCE}^{(0)}(\mathcal{D}_T) = 2n \cdot \mathbb{E}_{x_0, y_0 \sim \mathcal{D}_T} \left[e^{-\phi(x_0)^\intercal \psi(y_0)/\tau} \right],
$$

such that the downstream similarity score is computed as

$$
S_{(0)}(x_0, y_0) = \phi(x_0)^\top \psi(y_0).
$$

However, if we can estimate the mean of the unlabeled distribution $\mu_x$ and $\mu_y$ for $\phi(x_1)$ and $\psi(y_1)$, we can perform a first-order approximation of the distribution with $\widehat{P}(\phi(x_1)) = \mathbb{I}\{\phi(x_1) = \mu_x\}$ and $\widehat{P}(\psi(y_1)) = \mathbb{I}\{\psi(y_1) = \mu_y\}$, so that $\widehat{P}(\phi(x_1)), \widehat{P}(\psi(y_1))$ matches the true distribution in terms of the first-order moment. We are thus left with

$$
    \mathcal{L}_{NCE}^{(1)}(\mathcal{D}_T) = 2n \cdot \mathbb{E}_{x_0, y_0 \sim \mathcal{D}_T} \left[e^{\phi(x_0)^\intercal (\mu_y - \psi(y_0))/\tau} + e^{(\mu_x - \phi(x_0))^\intercal \psi(y_0)/\tau} \right].
$$

We show that this can be nicely represented as geometric mean, such that using monotonicity of exponentiation, we get a more accurate similarity score

$$
S_{(1)}(x_0, y_0) = (\phi(x_0) - \frac{1}{2}\mu_x)^\intercal (\psi(y_0) - \frac{1}{2}\mu_y).
$$

This translates directly to out implementation of DN, in which subtract off the mean of a test-time batch of image and text embeddings from each image and text embedding of the test set.

### Results

We present a representative subsection of our results. For the full list, please see our paper!

#### Cross-Modal Retrieval

We first present results on cross-modal retrieval performance on MSCOCO in the zero-shot setting for more CLIP variants. As can be seen from the bolded entries, adding DN improves retrieval accuracy across the board for CLIP and CLIP+TTA. Means for DN\* are estimated using 100 random unlabeled validation samples. Average recalls are calculated with 5 random seeds.

<table>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="6" style="text-align:center;">MSCOCO (5K test set)</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="3" style="text-align:center;">Image $\rightarrow$ Text</td>
        <td colspan="3" style="text-align:center;">Text $\rightarrow$ Image</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td style="text-align:center;">R@1</td>
        <td style="text-align:center;">R@5</td>
        <td style="text-align:center;">R@10</td>
        <td style="text-align:center;">R@1</td>
        <td style="text-align:center;">R@5</td>
        <td style="text-align:center;">R@10</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2103.00020" rel="noreferrer nofollow" target="_blank">CLIP</a></td>
        <td style="text-align:center;">$52.4$</td>
        <td style="text-align:center;">$76.0$</td>
        <td style="text-align:center;">$84.5$</td>
        <td style="text-align:center;">$30.2$</td>
        <td style="text-align:center;">$55.1$</td>
        <td style="text-align:center;">$66.4$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">$52.9$</td>
        <td style="text-align:center;">$76.4$</td>
        <td style="text-align:center;">$84.9$</td>
        <td style="text-align:center;">$32.1$</td>
        <td style="text-align:center;">$57.4$</td>
        <td style="text-align:center;">$68.3$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + <a href="https://arxiv.org/abs/2303.16730" rel="noreferrer nofollow" target="_blank">TTA</a> + DN*</td>
        <td style="text-align:center;">$\textbf{54.7}$</td>
        <td style="text-align:center;">$\textbf{77.8}$</td>
        <td style="text-align:center;">$\textbf{85.6}$</td>
        <td style="text-align:center;">$\textbf{33.8}$</td>
        <td style="text-align:center;">$\textbf{59.4}$</td>
        <td style="text-align:center;">$\textbf{70.1}$</td>
    </tr>

</table>

#### Zero-shot Classification

Next, we present zero-shot classification performance on ImageNet1K, Cifar100, and SUN397. Means for DN are estimated using 100 random unlabeled validation samples. Average accuracies and standard deviations are calculated with 5 random seeds.

<table>
    <tr>
        <td style="text-align:center;"></td>
        <td colspan="2" style="text-align:center;">ImageNet1K</td>
        <td colspan="2" style="text-align:center;">Cifar100</td>
        <td colspan="2" style="text-align:center;">SUN397</td>
    </tr>
    <tr>
        <td style="text-align:center;"></td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
        <td style="text-align:center;">Acc@1</td>
        <td style="text-align:center;">Acc@5</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2103.00020" rel="noreferrer nofollow" target="_blank">CLIP</a></td>
        <td style="text-align:center;">$61.0$</td>
        <td style="text-align:center;">$87.4$</td>
        <td style="text-align:center;">$63.9$</td>
        <td style="text-align:center;">$88.7$</td>
        <td style="text-align:center;">$56.1$</td>
        <td style="text-align:center;">$89.4$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.14169" rel="noreferrer nofollow" target="_blank">CALIP</a></td>
        <td style="text-align:center;">$61.2$</td>
        <td style="text-align:center;">$87.5$</td>
        <td style="text-align:center;">$64.2$</td>
        <td style="text-align:center;">$88.9$</td>
        <td style="text-align:center;">$56.1$</td>
        <td style="text-align:center;">$89.3$</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2209.07511" rel="noreferrer nofollow" target="_blank">TPT</a></td>
        <td style="text-align:center;">$\textbf{63.5}$</td>
        <td style="text-align:center;">$87.1$</td>
        <td style="text-align:center;">$65.2$</td>
        <td style="text-align:center;">$88.1$</td>
        <td style="text-align:center;">$\textbf{59.4}$</td>
        <td style="text-align:center;">$88.8$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">$61.7$</td>
        <td style="text-align:center;">$87.8$</td>
        <td style="text-align:center;">$65.1$</td>
        <td style="text-align:center;">$89.4$</td>
        <td style="text-align:center;">$57.3$</td>
        <td style="text-align:center;">$90.2$</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN *</td>
        <td style="text-align:center;">$63.2$</td>
        <td style="text-align:center;">$\textbf{88.9}$</td>
        <td style="text-align:center;">$\textbf{67.1}$</td>
        <td style="text-align:center;">$\textbf{90.7}$</td>
        <td style="text-align:center;">$58.1$</td>
        <td style="text-align:center;">$\textbf{90.7}$</td>
    </tr>
</table>

#### Image Captioning Metrics

We see that on image captioning, adding DN improves upon existing baselines on Flicker8k-Expert, Flickr8k-CF, and THumb datasets.

<table>
    <tr>
        <td colspan="2"></td>
        <td style="text-align:center;">Flickr8k-expert</td>
        <td style="text-align:center;">Flickr8k-cf</td>
        <td style="text-align:center;">THumb</td>
    </tr>
    <tr>
        <td colspan="2"></td>
        <td style="text-align:center;">$\tau_c$</td>
        <td style="text-align:center;">$\tau_b$</td>
        <td style="text-align:center;">$\tau_c$</td>
    </tr>
    <tr>
        <td rowspan="6">Ref-free</td>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td style="text-align:center;">51.4</td>
        <td style="text-align:center;">34.3</td>
        <td style="text-align:center;">19.9</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA</td>
        <td style="text-align:center;">51.9</td>
        <td style="text-align:center;">34.7</td>
        <td style="text-align:center;">20.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN</td>
        <td style="text-align:center;">53.6</td>
        <td style="text-align:center;">$\textbf{35.7}$</td>
        <td style="text-align:center;">23.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + TTA + DN*</td>
        <td style="text-align:center;">53.5</td>
        <td style="text-align:center;">35.5</td>
        <td style="text-align:center;">22.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN</td>
        <td style="text-align:center;">$\textbf{54.3}$</td>
        <td style="text-align:center;">35.4</td>
        <td style="text-align:center;">$\textbf{23.5}$</td>
    </tr>
    <tr  style="border-bottom:2px solid black">
        <td style="text-align:center;">CLIP + DN*</td>
        <td style="text-align:center;">53.2</td>
        <td style="text-align:center;">35.1</td>
        <td style="text-align:center;">22.2</td>
    </tr>
    <tr>
        <td rowspan="9">Ref-based</td>
        <td style="text-align:center;">BLEU-1</td>
        <td style="text-align:center;">32.3</td>
        <td style="text-align:center;">17.9</td>
        <td style="text-align:center;">11.1</td>
    </tr>
    <tr>
        <td style="text-align:center;">BLEU-4</td>
        <td style="text-align:center;">30.8</td>
        <td style="text-align:center;">16.9</td>
        <td style="text-align:center;">6.9</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/1411.5726" rel="noreferrer nofollow" target="_blank">CIDEr</a></td>
        <td style="text-align:center;">43.9</td>
        <td style="text-align:center;">24.6</td>
        <td style="text-align:center;">13.8</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/1908.02265" rel="noreferrer nofollow" target="_blank">ViLBERTScore-F</a></td>
        <td style="text-align:center;">50.1</td>
        <td style="text-align:center;">-</td>
        <td style="text-align:center;">-</td>
    </tr>
    <tr>
        <td style="text-align:center;"><a href="https://arxiv.org/abs/2104.08718" rel="noreferrer nofollow" target="_blank">CLIP-ref</a></td>
        <td style="text-align:center;">53.0</td>
        <td style="text-align:center;">36.4</td>
        <td style="text-align:center;">24.7</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN-ref</td>
        <td style="text-align:center;">$\textbf{55.3}$</td>
        <td style="text-align:center;">$\textbf{37.0}$</td>
        <td style="text-align:center;">25.8</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLIP + DN*-ref</td>
        <td style="text-align:center;">54.3</td>
        <td style="text-align:center;">36.9</td>
        <td style="text-align:center;">$\textbf{26.2}$</td>
    </tr>
</table>

### Authors

<div>
  <div style="text-align: left;">
  {% for person in site.data.authors %}
  <div class="person">
    <img src="{{ person.image }}" width=140 /><br>
    <a href="{{ person.url | relative_url }}">{{ person.name }}</a><br>
    <span>{{ person.title | replace: '&', '<br>' }}</span>
  </div>
  {% endfor %}
  </div>
</div>
\* indicates equal contribution.

### Citation

Need change after we update paper version on arXiv.

```

@misc{zhou2023distribution,
title={Distribution Normalization: An "Effortless" Test-Time Augmentation for Contrastively Learned Visual-language Models},
author={Yifei Zhou and Juntao Ren and Fengyu Li and Ramin Zabih and Ser-Nam Lim},
year={2023},
eprint={2302.11084},
archivePrefix={arXiv},
primaryClass={cs.LG}
}

```
