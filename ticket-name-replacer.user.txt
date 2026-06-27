// ==UserScript==
// @name         Ticket name replacer
// @namespace    local
// @version      1.0
// @description  チケット画面の名前表示を置き換える
// @match        https://livepocket.jp/*
// @grant        none
// ==/UserScript==

(function () {
  'use strict';

  const newLastName = 'XXXX';
  const newFirstName = 'XXXX';

  function replaceName() {
    const nameEl = document.querySelector('.my-ticket-info__name');
    if (!nameEl) return;

    const current = nameEl.textContent.replace(/\s+/g, '');
    const expected = `氏${newLastName}名${newFirstName}`;

    // すでに置き換え済みなら何もしない
    if (current === expected) return;

    const lastTitle = document.createElement('span');
    lastTitle.className = 'my-ticket-info__name-title';
    lastTitle.textContent = '氏';

    const firstTitle = document.createElement('span');
    firstTitle.className = 'my-ticket-info__name-title';
    firstTitle.textContent = '名';

    nameEl.replaceChildren(
      lastTitle,
      document.createTextNode(newLastName),
      firstTitle,
      document.createTextNode(newFirstName)
    );
  }

  replaceName();

  const observer = new MutationObserver(() => {
    replaceName();
  });

  observer.observe(document.body, {
    childList: true,
    subtree: true
  });
})();
