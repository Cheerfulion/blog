---
title: 【前端】实现CSDN等博客文章复制时带上自定义字符串的功能
tags:
  - 前端
abbrlink: bdc99a5d
date: 2021-05-24 09:36:17
---

````javascript
function getClipboardText() {
    var clipboardData = window.getSelection() || "";
    return clipboardData;
}

function setClipboardText(value) {
    var inputElement = document.createElement("input");

    inputElement.value = value;
    document.body.appendChild(inputElement);
    inputElement.select();
    document.execCommand("copy");
    inputElement.remove();
}

function onCopy() {
    document.removeEventListener("copy", onCopy);

    var text = getClipboardText();

    text =
        text +
        "\n" +
        "this is copy from ionluo, copyright all belong to ionluo.(^ ~ ^)";

    setClipboardText(text);

    document.addEventListener("copy", onCopy);
}

window.onload = function () {
    document.addEventListener("copy", onCopy);
};
````

