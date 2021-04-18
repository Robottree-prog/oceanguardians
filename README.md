# oceanguardians

Are you an interested naturalist?

Would you also like to be part of the team?

Can I or will I write more than your sales page is the real question. :)

Everytime.

Absolutely not- but thank you for the instructions. 

An ad through a widely used application ofcourse.

As fast the 2 clicks would allow. 

VERY VERY QUICKLY!

Have questions? Please send them to tasha.sylvia@outlook.com

<html class="" dir="ltr" bookmarkbarattached="false" lang="en"><head>
<meta charset="utf-8">
<title>New Tab</title>
<!-- Don't scale the viewport in either portrait or landscape mode.
     Note that this means apps will be reflowed when rotated (like iPad).
     If we wanted to maintain position we could remove 'maximum-scale' so
     that we'd zoom out in portrait mode, but then there would be a bunch
     of unusable space at the bottom.
-->
<meta name="viewport" content="user-scalable=no, width=device-width, maximum-scale=1.0">

<link id="themecss" rel="stylesheet" href="chrome://theme/css/new_tab_theme.css?1618717010960">
<script>// Copyright (c) 2013 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/**
 * @fileoverview Assertion support.
 */

/*
 * Verify |condition| is truthy and return |condition| if so.
 * @template T
 * @param {T} condition A condition to check for truthiness.  Note that this
 *     may be used to test whether a value is defined or not, and we don't want
 *     to force a cast to Boolean.
 * @param {string=} opt_message A message to show on failure.
 * @return {T} A non-null |condition|.
 * @closurePrimitive {asserts.truthy}
 * @suppress {reportUnknownTypes} because T is not sufficiently constrained.
 */
/* #export */ function assert(condition, opt_message) {
  if (!condition) {
    let message = 'Assertion failed';
    if (opt_message) {
      message = message + ': ' + opt_message;
    }
    const error = new Error(message);
    const global = function() {
      const thisOrSelf = this || self;
      /** @type {boolean} */
      thisOrSelf.traceAssertionsForTesting;
      return thisOrSelf;
    }();
    if (global.traceAssertionsForTesting) {
      console.warn(error.stack);
    }
    throw error;
  }
  return condition;
}

/**
 * Call this from places in the code that should never be reached.
 *
 * For example, handling all the values of enum with a switch() like this:
 *
 *   function getValueFromEnum(enum) {
 *     switch (enum) {
 *       case ENUM_FIRST_OF_TWO:
 *         return first
 *       case ENUM_LAST_OF_TWO:
 *         return last;
 *     }
 *     assertNotReached();
 *     return document;
 *   }
 *
 * This code should only be hit in the case of serious programmer error or
 * unexpected input.
 *
 * @param {string=} opt_message A message to show when this is hit.
 * @closurePrimitive {asserts.fail}
 */
/* #export */ function assertNotReached(opt_message) {
  assert(false, opt_message || 'Unreachable code hit');
}

/**
 * @param {*} value The value to check.
 * @param {function(new: T, ...)} type A user-defined constructor.
 * @param {string=} opt_message A message to show when this is hit.
 * @return {T}
 * @template T
 */
/* #export */ function assertInstanceof(value, type, opt_message) {
  // We don't use assert immediately here so that we avoid constructing an error
  // message if we don't have to.
  if (!(value instanceof type)) {
    assertNotReached(
        opt_message ||
        'Value ' + value + ' is not a[n] ' + (type.name || typeof type));
  }
  return value;
}
</script>
<script>// Copyright (c) 2021. Ocean Guardians All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// #import {assertInstanceof} from './assert.m.js';
// #import {dispatchSimpleEvent} from './cr.m.js';

/**
 * Alias for document.getElementById. Found elements must be HTMLElements.
 * @param {string} id The ID of the element to find.
 * @return {HTMLElement} The found element or null if not found.
 */
/* #export */ function $(id) {
  // Disable getElementById restriction here, since we are instructing other
  // places to re-use the $() that is defined here.
  // eslint-disable-next-line no-restricted-properties
  const el = document.getElementById(id);
  return el ? assertInstanceof(el, HTMLElement) : null;
}

// TODO(devlin): This should return SVGElement, but closure compiler is missing
// those externs.
/**
 * Alias for document.getElementById. Found elements must be SVGElements.
 * @param {string} id The ID of the element to find.
 * @return {Element} The found element or null if not found.
 */
/* #export */ function getSVGElement(id) {
  // Disable getElementById restriction here, since it is not suitable for SVG
  // elements.
  // eslint-disable-next-line no-restricted-properties
  const el = document.getElementById(id);
  return el ? assertInstanceof(el, Element) : null;
}

/**
 * @return {?Element} The currently focused element (including elements that are
 *     behind a shadow root), or null if nothing is focused.
 */
/* #export */ function getDeepActiveElement() {
  let a = document.activeElement;
  while (a && a.shadowRoot && a.shadowRoot.activeElement) {
    a = a.shadowRoot.activeElement;
  }
  return a;
}

// 

/**
 * @param {Node} el A node to search for ancestors with |className|.
 * @param {string} className A class to search for.
 * @return {Element} A node with class of |className| or null if none is found.
 */
/* #export */ function findAncestorByClass(el, className) {
  return /** @type {Element} */ (findAncestor(el, function(el) {
    return el.classList && el.classList.contains(className);
  }));
}

/**
 * Return the first ancestor for which the {@code predicate} returns true.
 * @param {Node} node The node to check.
 * @param {function(Node):boolean} predicate The function that tests the
 *     nodes.
 * @param {boolean=} includeShadowHosts
 * @return {Node} The found ancestor or null if not found.
 */
/* #export */ function findAncestor(node, predicate, includeShadowHosts) {
  while (node !== null) {
    if (predicate(node)) {
      break;
    }
    node = includeShadowHosts && node instanceof ShadowRoot ? node.host :
                                                              node.parentNode;
  }
  return node;
}

/**
 * Disables text selection and dragging, with optional callbacks to specify
 * overrides.
 * @param {function(Event):boolean=} opt_allowSelectStart Unless this function
 *    is defined and returns true, the onselectionstart event will be
 *    suppressed.
 * @param {function(Event):boolean=} opt_allowDragStart Unless this function
 *    is defined and returns true, the ondragstart event will be suppressed.
 */
/* #export */ function disableTextSelectAndDrag(
    opt_allowSelectStart, opt_allowDragStart) {
  // Disable text selection.
  document.onselectstart = function(e) {
    if (!(opt_allowSelectStart && opt_allowSelectStart.call(this, e))) {
      e.preventDefault();
    }
  };

  // Disable dragging.
  document.ondragstart = function(e) {
    if (!(opt_allowDragStart && opt_allowDragStart.call(this, e))) {
      e.preventDefault();
    }
  };
}

/**
 * Check the directionality of the page.
 * @return {boolean} True if Chrome is running an RTL UI.
 */
/* #export */ function isRTL() {
  return document.documentElement.dir === 'rtl';
}

/**
 * Get an element that's known to exist by its ID. We use this instead of just
 * calling getElementById and not checking the result because this lets us
 * satisfy the JSCompiler type system.
 * @param {string} id The identifier name.
 * @return {!HTMLElement} the Element.
 */
/* #export */ function getRequiredElement(id) {
  return assertInstanceof(
      $(id), HTMLElement, 'Missing required element: ' + id);
}

/**
 * Query an element that's known to exist by a selector. We use this instead of
 * just calling querySelector and not checking the result because this lets us
 * satisfy the JSCompiler type system.
 * @param {string} selectors CSS selectors to query the element.
 * @param {(!Document|!DocumentFragment|!Element)=} opt_context An optional
 *     context object for querySelector.
 * @return {!HTMLElement} the Element.
 */
/* #export */ function queryRequiredElement(selectors, opt_context) {
  const element = (opt_context || document).querySelector(selectors);
  return assertInstanceof(
      element, HTMLElement, 'Missing required element: ' + selectors);
}

/**
 * Creates a new URL which is the old URL with a GET param of key=value.
 * @param {string} url The base URL. There is not sanity checking on the URL so
 *     it must be passed in a proper format.
 * @param {string} key The key of the param.
 * @param {string} value The value of the param.
 * @return {string} The new URL.
 */
/* #export */ function appendParam(url, key, value) {
  const param = encodeURIComponent(key) + '=' + encodeURIComponent(value);

  if (url.indexOf('?') === -1) {
    return url + '?' + param;
  }
  return url + '&' + param;
}

/**
 * Creates an element of a specified type with a specified class name.
 * @param {string} type The node type.
 * @param {string} className The class name to use.
 * @return {Element} The created element.
 */
/* #export */ function createElementWithClassName(type, className) {
  const elm = document.createElement(type);
  elm.className = className;
  return elm;
}

/**
 * transitionend does not always fire (e.g. when animation is aborted
 * or when no paint happens during the animation). This function sets up
 * a timer and emulate the event if it is not fired when the timer expires.
 * @param {!HTMLElement} el The element to watch for transitionend.
 * @param {number=} opt_timeOut The maximum wait time in milliseconds for the
 *     transitionend to happen. If not specified, it is fetched from |el|
 *     using the transitionDuration style value.
 */
/* #export */ function ensureTransitionEndEvent(el, opt_timeOut) {
  if (opt_timeOut === undefined) {
    const style = getComputedStyle(el);
    opt_timeOut = parseFloat(style.transitionDuration) * 1000;

    // Give an additional 50ms buffer for the animation to complete.
    opt_timeOut += 50;
  }

  let fired = false;
  el.addEventListener('transitionend', function f(e) {
    el.removeEventListener('transitionend', f);
    fired = true;
  });
  window.setTimeout(function() {
    if (!fired) {
      cr.dispatchSimpleEvent(el, 'transitionend', true);
    }
  }, opt_timeOut);
}

/**
 * Alias for document.scrollTop getter.
 * @param {!HTMLDocument} doc The document node where information will be
 *     queried from.
 * @return {number} The Y document scroll offset.
 */
/* #export */ function scrollTopForDocument(doc) {
  return doc.documentElement.scrollTop || doc.body.scrollTop;
}

/**
 * Alias for document.scrollTop setter.
 * @param {!HTMLDocument} doc The document node where information will be
 *     queried from.
 * @param {number} value The target Y scroll offset.
 */
/* #export */ function setScrollTopForDocument(doc, value) {
  doc.documentElement.scrollTop = doc.body.scrollTop = value;
}

/**
 * Alias for document.scrollLeft getter.
 * @param {!HTMLDocument} doc The document node where information will be
 *     queried from.
 * @return {number} The X document scroll offset.
 */
/* #export */ function scrollLeftForDocument(doc) {
  return doc.documentElement.scrollLeft || doc.body.scrollLeft;
}

/**
 * Alias for document.scrollLeft setter.
 * @param {!HTMLDocument} doc The document node where information will be
 *     queried from.
 * @param {number} value The target X scroll offset.
 */
/* #export */ function setScrollLeftForDocument(doc, value) {
  doc.documentElement.scrollLeft = doc.body.scrollLeft = value;
}

/**
 * Replaces '&', '<', '>', '"', and ''' characters with their HTML encoding.
 * @param {string} original The original string.
 * @return {string} The string with all the characters mentioned above replaced.
 */
/* #export */ function HTMLEscape(original) {
  return original.replace(/&/g, '&amp;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;')
      .replace(/"/g, '&quot;')
      .replace(/'/g, '&#39;');
}

/**
 * Shortens the provided string (if necessary) to a string of length at most
 * |maxLength|.
 * @param {string} original The original string.
 * @param {number} maxLength The maximum length allowed for the string.
 * @return {string} The original string if its length does not exceed
 *     |maxLength|. Otherwise the first |maxLength| - 1 characters with '...'
 *     appended.
 */
/* #export */ function elide(original, maxLength) {
  if (original.length <= maxLength) {
    return original;
  }
  return original.substring(0, maxLength - 1) + '\u2026';
}

/**
 * Quote a string so it can be used in a regular expression.
 * @param {string} str The source string.
 * @return {string} The escaped string.
 */
/* #export */ function quoteString(str) {
  return str.replace(/([\\\.\+\*\?\[\^\]\$\(\)\{\}\=\!\<\>\|\:])/g, '\\$1');
}

/**
 * Calls |callback| and stops listening the first time any event in |eventNames|
 * is triggered on |target|.
 * @param {!EventTarget} target
 * @param {!Array<string>|string} eventNames Array or space-delimited string of
 *     event names to listen to (e.g. 'click mousedown').
 * @param {function(!Event)} callback Called at most once. The
 *     optional return value is passed on by the listener.
 */
/* #export */ function listenOnce(target, eventNames, callback) {
  if (!Array.isArray(eventNames)) {
    eventNames = eventNames.split(/ +/);
  }

  const removeAllAndCallCallback = function(event) {
    eventNames.forEach(function(eventName) {
      target.removeEventListener(eventName, removeAllAndCallCallback, false);
    });
    return callback(event);
  };

  eventNames.forEach(function(eventName) {
    target.addEventListener(eventName, removeAllAndCallCallback, false);
  });
}

/**
 * @param {!Event} e
 * @return {boolean} Whether a modifier key was down when processing |e|.
 */
/* #export */ function hasKeyModifiers(e) {
  return !!(e.altKey || e.ctrlKey || e.metaKey || e.shiftKey);
}

/**
 * @param {!Element} el
 * @return {boolean} Whether the element is interactive via text input.
 */
/* #export */ function isTextInputElement(el) {
  return el.tagName === 'INPUT' || el.tagName === 'TEXTAREA';
}
</script>
<script>
// Until themes can clear the cache, force-reload the theme stylesheet.
$('themecss').href = 'chrome://theme/css/new_tab_theme.css?' + Date.now();
</script>
<link rel="stylesheet" href="chrome://resources/css/text_defaults.css">
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

.bubble {
  position: absolute;
  white-space: normal;
  /* Height is dynamic, width fixed. */
  width: 300px;
  z-index: 9999;
}

.bubble-content {
  color: black;
  line-height: 150%;
  margin: 1px;
  padding: 8px 11px 12px;
  position: relative;
  z-index: 3;
}

/* When the close button is there, we need more padding on the right of the
 * bubble. */
.bubble-close:not([hidden]) ~ .bubble-content {
  padding-inline-end: 22px;
}

.bubble-close {
  height: 16px;
  position: absolute;
  right: 6px;
  top: 6px;
  width: 16px;
  z-index: 4;
}

html[dir='rtl'] .bubble-close {
  left: 6px;
  right: auto;
}

.bubble-close {
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAh0lEQVR42r2RgQbDMBRFH7c3EET6K8UEDyCE1QDT//+Q7Vm81R4MZgqXc+hJIo8v3w8FOVB9Y5UjCIZvS0eZGHfs0j8E1KWzUVFQDV+pzOEXKNR0MWVg2Mqx4VQam+MoVAyDW9oYhXeaCw0lCFhnWsrMbK+WEo+5zzRvEY0X1ZnPXFHff3isJx/SxkVFQYa2AAAAAElFTkSuQmCC) 1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAA9UlEQVR4Xu3UsWrCUByH0fMEouiuhrg4xohToJVGH0CHLBncEwfx/VvIFHLJBWmHDvKbv7PcP9f3L/fXwBsApZSRpUpEgbOnxwiReng6x4AvjdrNXRLkibubWqMcB9Yujk7qjhjmtZOji/U4wELuoBwQXa50kFsQA5jK+kQ/l5kSA4ZEK5Fo+3kcCIlGM8ijQEhUqkEeBUKiUPTyl4C5vZ1cbmdv/iqwclXY6aZwtXoFSLQqhVwmkytUWglxAMG7T0yCu4gD0v7ZBKeVxoEwFxIxYBPmIWEzDnyEeUj4HAfYdvmMcGYdsSUGsOzlIbHEv/uV38APrreiBRBIs3QAAAAASUVORK5CYII=) 2x);
}

.bubble-close:hover {
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAqUlEQVR4XqWRMQqEMBBF/1E8Ra6x6V3FRnS9QbCxtJg6Z7CzE9lTiIXXyUb3C8EULixDIMM8Zt4kcDfxM5A45U+cgeXnC1tREgkzAgob3hiq3CUHvGLG4FTQoSgxQGDrzN8WTLBGnx2IVDksen9GH7Z9hA5E6uxABMJyCHDMCEGHzugLQPPlBCBNGq+5YtpnGw1Bv+te15ypljTpVzdak5Opy+z+qf//zQ+Lg+07ay5KsgAAAABJRU5ErkJggg==) 1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAB4UlEQVR42u2VsWoCQRBAh+MUFP0C1V9QD4NEOxs9xBQHQVCwSJFWVBAtBNXCxk6wTkBJYUTwEwQLC61E8QP0NzZzt5g5726DkC7EYWHZ8T3WndkV2C/jLwn4hwVYBIdLn9vkLp79QcBCTDMiy3w2gQ9XeTYkEHA8vqj2rworXu3HF1YFfSWgp5QFnKVLvYvzDEKEZ5hW70oXOCtcEbQLIkx7+IQtfMBSOjU6XEF4oyOdYInZbXyOuajjDlpNeQgleIUJKUz4BDMledhqOu/AzVSmzZ49CUjCC0yvim98iqtJT2L2jKsqczsdok9XrHNexaww415lnTNwn6CM/KxJIR8bnUZHPhLO6yMoIyk2pNjLewFuE5AiY1KMMQx8Q7hQYFek4AkjxXFe1rsF84I/BTFQMGL+1Lxwl4DwdtM1gjwKohgxyLtG7SYpxALqugOMcfOKN+bFXeBsLB1uulNcRqq7/tt36k41zoL6QlxGjtd6lrahiqCi1iOFYyvXuxY8yzK33VnvUivbLlOlj/jktm0s3YnXrNIXXufHNxuOGasi8S68zkwrlnV8ZcJJsTIUxbLgQcFZWE8N0gau2p40VVcM0gYeFpSRK6445UhBuKiRgiyKw+34rLt59nb1/7+RwReVkaFtqvNBuwAAAABJRU5ErkJggg==) 2x);
}

.bubble-close:active {
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAQklEQVR4AWP4TwBSTQGDHcMZIIYAKA9VwRkwtINJgyCaCTAlCBaKAoQ+hFmoCqBKENKkK8C0gpAjCXuTyICiQ2QBAPSwyG3ByZlCAAAAAElFTkSuQmCC) 1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAA/ElEQVR4Xu3UsWrCUBiG4efGlIBoIMFbcnYolYJ3pg4iKGrGYFTRwaUFhYAekiDt0EG++X2W83N8/3J/DbwBMJJSsdQItcDY1VlCOImzq3Ed8OmicHASB3ns5KBw8VUNpDJrW7uAiJ3sbK1l0mqArpmFTUlQ5jYWZrrUAUSmT0SZm4qoA56JvVhs/5g3A7RLolA85A1ASOTye65NMxASK6syfxGITMzvMxG9CvRkliWwlOm9AsSOcitzU1NzK7mjuBkQvHtLK7iLBiB5PhttJSGpB8I8vM6kDuiHeUjoVwMfYR4SRtUAw1veIZzOjRhSBzCoyKFjgH/3K7+BHzg+Cgw0eSW3AAAAAElFTkSuQmCC) 2x);
}

.bubble-shadow {
  bottom: 0;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
  z-index: 1;
}

.bubble-arrow {
  box-shadow: 1px 1px 6px rgba(0, 0, 0, 0.15);
  height: 15px;
  position: absolute;
  transform: rotate(45deg);
  width: 15px;
  z-index: 2;
}

.bubble-content,
.bubble-arrow {
  background: white;
}

.bubble-shadow,
.bubble-arrow {
  border: 1px solid rgba(0, 0, 0, 0.3);
}

.bubble-shadow,
.bubble-content {
  border-radius: 6px;
  box-sizing: border-box;
}

.auto-close-bubble {
  position: fixed;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

cr-menu {
  -webkit-box-shadow: 0 2px 4px rgba(0, 0, 0, .50);
  background: white;
  border-radius: 2px;
  color: black;
  cursor: default;
  left: 0;
  margin: 0;
  outline: 1px solid rgba(0, 0, 0, 0.2);
  padding: 8px 0;
  position: fixed;
  white-space: nowrap;
  z-index: 3;
}

cr-menu:not(.decorated) {
  display: none;
}

cr-menu > * {
  box-sizing: border-box;
  display: block;
  margin: 0;
  text-align: start;
  width: 100%;
}

cr-menu > :not(hr) {
  -webkit-appearance: none;
  background: transparent;
  border: 0;
  color: black;
  font: inherit;
  line-height: 18px;
  outline: none;
  overflow: hidden;
  padding: 0 19px;
  text-overflow: ellipsis;
}

cr-menu > hr {
  background: -webkit-linear-gradient(left,
                                      rgba(0, 0, 0, .10),
                                      rgba(0, 0, 0, .02) 96%);
  border: 0;
  height: 1px;
  margin: 8px 0;
}

cr-menu > [disabled] {
  color: rgba(0, 0, 0, .3);
}

cr-menu > [hidden] {
  display: none;
}

cr-menu > :not(hr):-webkit-any([selected], :active) {
  background-color: rgba(0, 0, 0, .06);
}

cr-menu > [checked]::before {
  content: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAJCAQAAABKmM6bAAAAPUlEQVR4AWNAA8YMgugC7xiUGICixkgC/0FCSkCGMVzgHUijEphRDhGA6BAEc/5DBRBmIAQQgneRBP5jQAAe7CCchpa/PQAAAABJRU5ErkJggg==) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAASCAQAAAD8x0bcAAAAlElEQVR4Xq3KoW1CURQA0EPNs60lsAEToNolYAOwhD2aWroBLAGKCdgAggX7DNyaL3pD+P2iOfaIDronz/R8tieKtWhPr7aiPQ0cRE4rJZWRo8iJsNcH8O7SlLvl7xTOxmCiNqWaklOoZhZuTbn6QE6ZkxE5zdVUDobkBGNn0dh54zFB314IG4VnieLblxceU7t/Tj+dadMtVYp4hwAAAABJRU5ErkJggg==) 2x);
  display: inline-block;
  height: 9px;
  margin: 0 5px;
  width: 9px;
}

cr-menu > [checked] {
  padding-inline-start: 0;
}

cr-menu > [selected][checked]:active::before {
  content: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAJCAQAAABKmM6bAAAAQklEQVR4Xl3LQQ0AIAwDwFmYBSxMHGqxgIVSmjSEpZ/ulgb+FLLDxggkywNcGixlICZJZQr05FAHDCRPDCLhEph6DuGWaFS/FhbPAAAAAElFTkSuQmCC) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABIAAAASCAQAAAD8x0bcAAAAkUlEQVR42rXRsRHBYBgG4NCkpXVskAlSsQQb0Dp7OC0bsARVJsgGXFraNDzuFM4fhILvbb7iqd438kV+gBrmn1BsTT1q2VKPunJCtBQHJLGnish07qTvCLiYPSIK6e0fKgGlkShElMamzoCTQdjT8x0k1TInyoDkeq8aTxUAdtrvZunIwEZct11sZaH51cD/RFfxwIiG5OFb1QAAAABJRU5ErkJggg==) 2x);
}

/* TODO(zvorygin) menu > [shortcutText]::after - this selector is much better,
 * but it's buggy in current webkit revision, so I have to use [showShortcuts].
 */
cr-menu[showShortcuts] > ::after {
  color: #999;
  content: attr(shortcutText);
  float: right;
  padding-inline-start: 30px;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

/* NOTE: If you are using the drop-down style, you must first call
 * MenuButton.createDropDownArrows() to initialize the CSS canvases that
 * contain the arrow images. */

button.menu-button.drop-down {
  background: white url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9IiNjMGMzYzYiIHdpZHRoPSI2IiBoZWlnaHQ9IjMiPjxwYXRoIGQ9Ik0wIDBoNkwzIDMiLz48L3N2Zz4=) no-repeat center 4px;
  border: 1px solid rgb(192, 195, 198);
  border-radius: 2px;
  height: 12px;
  margin: 0 5px;
  padding: 0;
  position: relative;
  top: 1px;
  width: 12px;
}

button.menu-button.drop-down:hover {
  background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI2IiBoZWlnaHQ9IjMiPjxwYXRoIGQ9Ik0wIDBoNkwzIDMiLz48L3N2Zz4=);
  border-color: rgb(48, 57, 66);
}

button.menu-button.drop-down[menu-shown],
button.menu-button.drop-down:focus {
  background-color: rgb(48, 57, 66);
  background-image: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9IiNmZmYiIHdpZHRoPSI2IiBoZWlnaHQ9IjMiPjxwYXRoIGQ9Ik0wIDBoNkwzIDMiLz48L3N2Zz4=);
  border-color: rgb(48, 57, 66);
}

button.menu-button.using-mouse {
  outline: none;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

/* This file defines styles for form controls. The order of rule blocks is
 * important as there are some rules with equal specificity that rely on order
 * as a tiebreaker. These are marked with OVERRIDE. */

/* Copyright 2015 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

[is='action-link'] {
  cursor: pointer;
  display: inline-block;
  text-decoration: none;
}

[is='action-link']:hover {
  text-decoration: underline;
}

[is='action-link']:active {
  color: rgb(5, 37, 119);
  text-decoration: underline;
}

[is='action-link'][disabled] {
  color: #999;
  cursor: default;
  pointer-events: none;
  text-decoration: none;
}

[is='action-link'].no-outline {
  outline: none;
}


/* Default state **************************************************************/

:-webkit-any(button,
             input[type='button'],
             input[type='submit']):not(.custom-appearance),
select,
input[type='checkbox'],
input[type='radio'] {
  -webkit-appearance: none;
  background-image: -webkit-linear-gradient(#ededed, #ededed 38%, #dedede);
  border: 1px solid rgba(0, 0, 0, 0.25);
  border-radius: 2px;
  box-shadow: 0 1px 0 rgba(0, 0, 0, 0.08),
      inset 0 1px 2px rgba(255, 255, 255, 0.75);
  color: #444;
  font: inherit;
  margin: 0 1px 0 0;
  outline: none;
  text-shadow: 0 1px 0 rgb(240, 240, 240);
  user-select: none;
}

:-webkit-any(button,
             input[type='button'],
             input[type='submit']):not(.custom-appearance),
select {
  min-height: 2em;
  min-width: 4em;
  padding-bottom: 1px;

  /* The following platform-specific rule is necessary to get adjacent
   * buttons, text inputs, and so forth to align on their borders while also
   * aligning on the text's baselines. */
  padding-bottom: 2px;

  padding-top: 1px;
}

:-webkit-any(button,
             input[type='button'],
             input[type='submit']):not(.custom-appearance) {
  padding-inline-end: 10px;
  padding-inline-start: 10px;
}

select {
  -webkit-appearance: none;
  /* OVERRIDE */
  background-image: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAICAQAAACxSAwfAAAAUUlEQVR4AWP4TxREZkYxpKHAKKzKEhOZvyG4zN8SE7Eq+6+wYCHbTwiT7eeChf8VsFsKVQhTxIDDbVCFfF8ginApgyp82wRShEcZVJIVzoJDAGqrgIJGRl20AAAAAElFTkSuQmCC) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAQAAAA/1a6rAAAAQUlEQVR4Xu3MsQnAMBAEMI1+myf9gw0+3ASCenmu+mQn2yGn3S4Mp906DEW3CEPfzTD03QxD380w3OmIUHe9v+u9QwAt93yns5cAAAAASUVORK5CYII=) 2x),
      -webkit-linear-gradient(#ededed, #ededed 38%, #dedede);
  background-position: right center;
  background-repeat: no-repeat;
  padding-inline-end: 24px;
  padding-inline-start: 10px;
}

html[dir='rtl'] select {
  background-position: center left;
}

input[type='checkbox'] {
  height: 13px;
  position: relative;
  vertical-align: middle;
  width: 13px;
}

input[type='radio'] {
  /* OVERRIDE */
  border-radius: 100%;
  height: 15px;
  position: relative;
  vertical-align: middle;
  width: 15px;
}

/* TODO(estade): add more types here? */
input[type='number'],
input[type='password'],
input[type='search'],
input[type='text'],
input[type='url'],
input:not([type]),
textarea {
  border: 1px solid #bfbfbf;
  border-radius: 2px;
  box-sizing: border-box;
  color: #444;
  font: inherit;
  margin: 0;
  /* Use min-height to accommodate addditional padding for touch as needed. */
  min-height: 2em;
  outline: none;
  padding: 3px;

  /* For better alignment between adjacent buttons and inputs. */
  padding-bottom: 4px;

}

input[type='search'] {
  -webkit-appearance: textfield;
  /* NOTE: Keep a relatively high min-width for this so we don't obscure the end
   * of the default text in relatively spacious languages (i.e. German). */
  min-width: 160px;
}

/* Checked ********************************************************************/

input[type='checkbox']:checked::before {
  background-image: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAsAAAALCAQAAAADpb+tAAAAZ0lEQVR4AWNAA2xAiAXEM8xiMEAXVGJYz7AZCFEkmBi6wYKtEC4/gxqY9gILrmYQhwiXMWxkiAVyVoOFfSCCpkAmCK4Fk+1QA4GqekECUAMkka0KY9gIFvZDd5oawwyGBqACdIDqOwAQzBnTWnnU+gAAAABJRU5ErkJggg==) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABYAAAAWCAQAAABuvaSwAAAAvElEQVR4XrXPMUrDYBzG4UeRZnAQnFxq3XT3AsVABm8QPIHQIeAJuoqb2s1BcHAIin4HVLqEvx9NQgb5rc/wvn4mNBUbqlKDcezCp6Qexxx7lbapx/CBe6mrHsYrKXQ7hKtIre1nOD/W9eiQiK80inis680JEc+1kien+TEfzom4sJG2aZXxmG9LIqaRerohx6V2J72zl2NY2OTUgxm7MEU25sURfZg4590Zw5iFZ8mXS0ZwN+eaPjyh/8O/H7bzPJ5NOo0AAAAASUVORK5CYII=) 2x);
  background-size: 100% 100%;
  content: '';
  display: block;
  height: 100%;
  user-select: none;
  width: 100%;
}

input[type='radio']:checked::before {
  background-color: #666;
  border-radius: 100%;
  bottom: 3px;
  content: '';
  display: block;
  left: 3px;
  position: absolute;
  right: 3px;
  top: 3px;
}


/* Hover **********************************************************************/

:enabled:hover:-webkit-any(
    select,
    input[type='checkbox'],
    input[type='radio'],
    :-webkit-any(
        button,
        input[type='button'],
        input[type='submit']):not(.custom-appearance)) {
  background-image: -webkit-linear-gradient(#f0f0f0, #f0f0f0 38%, #e0e0e0);
  border-color: rgba(0, 0, 0, 0.3);
  box-shadow: 0 1px 0 rgba(0, 0, 0, 0.12),
      inset 0 1px 2px rgba(255, 255, 255, 0.95);
  color: black;
}

:enabled:hover:-webkit-any(select) {
  /* OVERRIDE */
  background-image: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAICAQAAACxSAwfAAAAUUlEQVR4AWP4TxREZkYxpKHAKKzKEhOZvyG4zN8SE7Eq+6+wYCHbTwiT7eeChf8VsFsKVQhTxIDDbVCFfF8ginApgyp82wRShEcZVJIVzoJDAGqrgIJGRl20AAAAAElFTkSuQmCC) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAQAAAA/1a6rAAAAQUlEQVR4Xu3MsQnAMBAEMI1+myf9gw0+3ASCenmu+mQn2yGn3S4Mp906DEW3CEPfzTD03QxD380w3OmIUHe9v+u9QwAt93yns5cAAAAASUVORK5CYII=) 2x),
      -webkit-linear-gradient(#f0f0f0, #f0f0f0 38%, #e0e0e0);
}


/* Active *********************************************************************/

:enabled:active:-webkit-any(
    select,
    input[type='checkbox'],
    input[type='radio'],
    :-webkit-any(
        button,
        input[type='button'],
        input[type='submit']):not(.custom-appearance)) {
  background-image: -webkit-linear-gradient(#e7e7e7, #e7e7e7 38%, #d7d7d7);
  box-shadow: none;
  text-shadow: none;
}

:enabled:active:-webkit-any(select) {
  /* OVERRIDE */
  background-image: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAICAQAAACxSAwfAAAAUUlEQVR4AWP4TxREZkYxpKHAKKzKEhOZvyG4zN8SE7Eq+6+wYCHbTwiT7eeChf8VsFsKVQhTxIDDbVCFfF8ginApgyp82wRShEcZVJIVzoJDAGqrgIJGRl20AAAAAElFTkSuQmCC) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACcAAAAQCAQAAAA/1a6rAAAAQUlEQVR4Xu3MsQnAMBAEMI1+myf9gw0+3ASCenmu+mQn2yGn3S4Mp906DEW3CEPfzTD03QxD380w3OmIUHe9v+u9QwAt93yns5cAAAAASUVORK5CYII=) 2x),
      -webkit-linear-gradient(#e7e7e7, #e7e7e7 38%, #d7d7d7);
}

/* Disabled *******************************************************************/

:disabled:-webkit-any(
    button,
    input[type='button'],
    input[type='submit']):not(.custom-appearance),
select:disabled {
  background-image: -webkit-linear-gradient(#f1f1f1, #f1f1f1 38%, #e6e6e6);
  border-color: rgba(80, 80, 80, 0.2);
  box-shadow: 0 1px 0 rgba(80, 80, 80, 0.08),
      inset 0 1px 2px rgba(255, 255, 255, 0.75);
  color: #aaa;
}

select:disabled {
  /* OVERRIDE */
  background-image: -webkit-image-set(url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABMAAAAICAQAAACxSAwfAAAASUlEQVR4AWP4TxREZkYxpKHAKKzKEhMb/iPDxESsyv4rLFiIULRg4X8F7JaCFSIUMeBwG1QhTBEuZVCFb5tAivAog0qywllwCAAavoiLhz+UlAAAAABJRU5ErkJggg==) 1x, url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACYAAAAQCAQAAADQF8WVAAAARElEQVR4Xu3MsQ0AIAwEsYx+m4fySsgLOuTe1Re9z4De4DzbdVDnmZ0ENcrsZJVkdoIKMzurMLOzSjNhlWfCapBlfpZbeMFeGdxKIEQAAAAASUVORK5CYII=) 2x),
      -webkit-linear-gradient(#f1f1f1, #f1f1f1 38%, #e6e6e6);
}

input:disabled:-webkit-any([type='checkbox'],
                           [type='radio']) {
  opacity: .75;
}

input:disabled:-webkit-any([type='password'],
                           [type='search'],
                           [type='text'],
                           [type='url'],
                           :not([type])) {
  color: #999;
}

/* Focus **********************************************************************/

:enabled:focus:-webkit-any(
    select,
    input[type='checkbox'],
    input[type='number'],
    input[type='password'],
    input[type='radio'],
    input[type='search'],
    input[type='text'],
    input[type='url'],
    input:not([type]),
    :-webkit-any(
         button,
         input[type='button'],
         input[type='submit']):not(.custom-appearance)) {
  /* We use border color because it follows the border radius (unlike outline).
   * This is particularly noticeable on mac. */
  border-color: rgb(77, 144, 254);
  outline: none;
  /* OVERRIDE */
  transition: border-color 200ms;
}

/* Checkbox/radio helpers ******************************************************
 *
 * .checkbox and .radio classes wrap labels. Checkboxes and radios should use
 * these classes with the markup structure:
 *
 *   <div class="checkbox">
 *     <label>
 *       <input type="checkbox">
 *       <span>
 *     </label>
 *   </div>
 */

:-webkit-any(.checkbox, .radio) label {
  /* Don't expand horizontally: <http://crbug.com/112091>. */
  align-items: center;
  display: inline-flex;
  padding-bottom: 7px;
  padding-top: 7px;
  user-select: none;
}

:-webkit-any(.checkbox, .radio) label input {
  flex-shrink: 0;
}

:-webkit-any(.checkbox, .radio) label input ~ span {
  /* Make sure long spans wrap at the same horizontal position they start. */
  display: block;
  margin-inline-start: 0.6em;
}

:-webkit-any(.checkbox, .radio) label:hover {
  color: black;
}

label > input:disabled:-webkit-any([type='checkbox'], [type='radio']) ~ span {
  color: #999;
}

extensionview {
  display: inline-block;
  height: 300px;
  width: 300px;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

.app {
  outline: none;
  position: absolute;
  text-align: center;
}

.app-contents {
  transition: transform 100ms;
}

.app-contents:active:not(.suppress-active),
.app:not(.click-focus):focus .app-contents:not(.suppress-active),
.drag-representation:not(.placing) .app-contents {
  transform: scale(1.1);
}

/* Don't animate the initial scaling.  */
.app-contents:active:not(.suppress-active),
/* Active gets applied right before .suppress-active, so to avoid flicker
 * we need to make the scale go back to normal without an animation. */
.app-contents.suppress-active {
  transition-duration: 0ms;
}

.app-contents > span {
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.app-img-container {
  /* -webkit-mask-image set by JavaScript to the image source. */
  -webkit-mask-size: 100% 100%;
  margin-left: auto;
  margin-right: auto;
}

.app-img-container > * {
  height: 100%;
  width: 100%;
}

.app-icon-div {
  -webkit-box-align: center;
  -webkit-box-pack: center;
  background-color: white;
  border: 1px solid #d5d5d5;
  border-radius: 5px;
  display: -webkit-box;
  margin-left: auto;
  margin-right: auto;
  position: relative;
  vertical-align: middle;
  z-index: 0;
}

.app-icon-div .app-img-container {
  bottom: 10px;
  left: 10px;
  position: absolute;
}

.app-icon-div .color-stripe {
  border-bottom-left-radius: 5px 5px;
  border-bottom-right-radius: 5px 5px;
  bottom: 0;
  height: 3px;
  opacity: 1.0;
  position: absolute;
  width: 100%;
  z-index: 100;
}

.app-context-menu > button:first-child {
  font-weight: bold;
}

.app-context-menu {
  z-index: 1000;
}

.app-context-menu > [checked]::before {
  height: 5px;
}

.launch-click-target {
  cursor: pointer;
}

.app-img-container > img:first-child {
  display: block;
}

.app .invisible {
  visibility: hidden;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

/* TODO(estade): handle overflow better? I tried overflow-x: hidden and
 * overflow-y: visible (for the new dot animation), but this makes a scroll
 * bar appear */
#dot-list {
  /* Expand to take up all available horizontal space.  */
  -webkit-box-flex: 1;
  /* Center child dots. */
  -webkit-box-pack: center;
  display: -webkit-flex;
  height: 100%;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

html.starting-up #dot-list {
  display: none;
}

.dot {
  box-sizing: border-box;
  cursor: pointer;
  /* max-width: Set in new_tab.js. See measureNavDots() */
  margin-inline-end: 10px;
  outline: none;
  padding-inline-start: 2px;
  text-align: left;
  transition: margin-inline-end 250ms, max-width 250ms, opacity 250ms;
}

.dot:last-child {
  margin-inline-end: 0;
}

.dot:only-of-type {
  cursor: default;
  opacity: 0;
  pointer-events: none;
}

.dot.small {
  margin-inline-end: 0;
  max-width: 0;
}

.dot .selection-bar {
  border-bottom: 5px solid;
  border-color: rgba(0, 0, 0, 0.1);
  height: 10px;
  transition: border-color 200ms;
}

.dot input {
  -webkit-appearance: none;
  background-color: transparent;
  border: none;
  cursor: inherit;
  font: inherit;
  height: auto;
  margin-inline-start: 2px;
  margin-top: 2px;
  padding: 2px 1px;
  transition: color 200ms;
  width: 90%;
}

.dot input:focus {
  cursor: auto;
}

/* Everything below here should be themed but we don't have appropriate colors
 * yet. */
.dot input {
  color: #b2b2b2;
}

.dot:focus input,
.dot:hover input,
.dot.selected input {
  color: #7f7f7f;
}

.dot:focus .selection-bar,
.dot:hover .selection-bar,
.dot.drag-target .selection-bar {
  border-color: #b2b2b2;
}

.dot.selected .selection-bar {
  border-color: #7f7f7f;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

html {
  /* It's necessary to put this here instead of in body in order to get the
     background-size of 100% to work properly */
  height: 100%;
  overflow: hidden;
}

body {
  /* Don't highlight links when they're tapped. Safari has bugs here that
     show up as flicker when dragging in some situations */
  -webkit-tap-highlight-color: transparent;
  background-size: auto 100%;
  margin: 0;
  /* Don't allow selecting text - can occur when dragging */
  user-select: none;
}

/* [hidden] does display:none, but its priority is too low in some cases. */
[hidden] {
  display: none !important;
}

.close-button {
  background: no-repeat;
  background-color: transparent;
  /* TODO(estade): this should animate between states. */
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAh0lEQVR42r2RgQbDMBRFH7c3EET6K8UEDyCE1QDT//+Q7Vm81R4MZgqXc+hJIo8v3w8FOVB9Y5UjCIZvS0eZGHfs0j8E1KWzUVFQDV+pzOEXKNR0MWVg2Mqx4VQam+MoVAyDW9oYhXeaCw0lCFhnWsrMbK+WEo+5zzRvEY0X1ZnPXFHff3isJx/SxkVFQYa2AAAAAElFTkSuQmCC) 1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAA9UlEQVR4Xu3UsWrCUByH0fMEouiuhrg4xohToJVGH0CHLBncEwfx/VvIFHLJBWmHDvKbv7PcP9f3L/fXwBsApZSRpUpEgbOnxwiReng6x4AvjdrNXRLkibubWqMcB9Yujk7qjhjmtZOji/U4wELuoBwQXa50kFsQA5jK+kQ/l5kSA4ZEK5Fo+3kcCIlGM8ijQEhUqkEeBUKiUPTyl4C5vZ1cbmdv/iqwclXY6aZwtXoFSLQqhVwmkytUWglxAMG7T0yCu4gD0v7ZBKeVxoEwFxIxYBPmIWEzDnyEeUj4HAfYdvmMcGYdsSUGsOzlIbHEv/uV38APrreiBRBIs3QAAAAASUVORK5CYII=) 2x);
  border: 0;
  cursor: default;
  display: inline-block;
  height: 16px;
  padding: 0;
  width: 16px;
}

.close-button:hover,
.close-button:focus {
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAqUlEQVR4XqWRMQqEMBBF/1E8Ra6x6V3FRnS9QbCxtJg6Z7CzE9lTiIXXyUb3C8EULixDIMM8Zt4kcDfxM5A45U+cgeXnC1tREgkzAgob3hiq3CUHvGLG4FTQoSgxQGDrzN8WTLBGnx2IVDksen9GH7Z9hA5E6uxABMJyCHDMCEGHzugLQPPlBCBNGq+5YtpnGw1Bv+te15ypljTpVzdak5Opy+z+qf//zQ+Lg+07ay5KsgAAAABJRU5ErkJggg==) 1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAB4UlEQVR42u2VsWoCQRBAh+MUFP0C1V9QD4NEOxs9xBQHQVCwSJFWVBAtBNXCxk6wTkBJYUTwEwQLC61E8QP0NzZzt5g5726DkC7EYWHZ8T3WndkV2C/jLwn4hwVYBIdLn9vkLp79QcBCTDMiy3w2gQ9XeTYkEHA8vqj2rworXu3HF1YFfSWgp5QFnKVLvYvzDEKEZ5hW70oXOCtcEbQLIkx7+IQtfMBSOjU6XEF4oyOdYInZbXyOuajjDlpNeQgleIUJKUz4BDMledhqOu/AzVSmzZ49CUjCC0yvim98iqtJT2L2jKsqczsdok9XrHNexaww415lnTNwn6CM/KxJIR8bnUZHPhLO6yMoIyk2pNjLewFuE5AiY1KMMQx8Q7hQYFek4AkjxXFe1rsF84I/BTFQMGL+1Lxwl4DwdtM1gjwKohgxyLtG7SYpxALqugOMcfOKN+bFXeBsLB1uulNcRqq7/tt36k41zoL6QlxGjtd6lrahiqCi1iOFYyvXuxY8yzK33VnvUivbLlOlj/jktm0s3YnXrNIXXufHNxuOGasi8S68zkwrlnV8ZcJJsTIUxbLgQcFZWE8N0gau2p40VVcM0gYeFpSRK6445UhBuKiRgiyKw+34rLt59nb1/7+RwReVkaFtqvNBuwAAAABJRU5ErkJggg==) 2x);
}

.close-button:active {
  background-image: -webkit-image-set(
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAQAAAC1+jfqAAAAQklEQVR4AWP4TwBSTQGDHcMZIIYAKA9VwRkwtINJgyCaCTAlCBaKAoQ+hFmoCqBKENKkK8C0gpAjCXuTyICiQ2QBAPSwyG3ByZlCAAAAAElFTkSuQmCC)
          1x,
      url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAQAAADZc7J/AAAA/ElEQVR4Xu3UsWrCUBiG4efGlIBoIMFbcnYolYJ3pg4iKGrGYFTRwaUFhYAekiDt0EG++X2W83N8/3J/DbwBMJJSsdQItcDY1VlCOImzq3Ed8OmicHASB3ns5KBw8VUNpDJrW7uAiJ3sbK1l0mqArpmFTUlQ5jYWZrrUAUSmT0SZm4qoA56JvVhs/5g3A7RLolA85A1ASOTye65NMxASK6syfxGITMzvMxG9CvRkliWwlOm9AsSOcitzU1NzK7mjuBkQvHtLK7iLBiB5PhttJSGpB8I8vM6kDuiHeUjoVwMfYR4SRtUAw1veIZzOjRhSBzCoyKFjgH/3K7+BHzg+Cgw0eSW3AAAAAElFTkSuQmCC)
          2x);
}

[is='action-link'] {
  margin-inline-start: 0.5em;
}

#card-slider-frame {
  /* Must match #footer height. */
  bottom: 50px;
  overflow: hidden;
  /* We want this to fill the window except for the region used
   * by footer. */
  position: fixed;
  top: 0;
  width: 100%;
}

#page-list {
  /* fill the apps-frame */
  display: -webkit-box;
  height: 100%;
}

#attribution {
  bottom: 0;
  margin-left: 8px;
  /* Leave room for the scrollbar. */
  margin-right: 13px;
  position: absolute;
  z-index: -5;
}

html[dir='rtl'] #attribution {
  left: 0;
  right: auto;
  text-align: right;
}

#attribution > span {
  display: block;
}

#footer {
  background-color: white;
  bottom: 0;
  color: #666;
  font-size: 0.9em;
  font-weight: bold;
  overflow: hidden;
  position: fixed;
  width: 100%;
  z-index: 5;
}

/* TODO(estade): remove this border hack and replace with a webkit-gradient
 * border-image on #footer once WebKit supports border-image-slice.
 * See https://bugs.webkit.org/show_bug.cgi?id=20127 */
#footer-border {
  height: 1px;
}

#footer-content {
  -webkit-align-items: center;
  -webkit-justify-content: space-between;
  display: -webkit-flex;
  height: 49px;
}

#footer-content > * {
  margin: 0 9px;
}

#logo-img {
  display: inline-block;
  margin-top: 4px;
  overflow: hidden;
  position: relative;
}

.starting-up * {
  transition: none !important;
}

/* Login Status. **************************************************************/

#login-container {
  background: transparent none;
  border: none;
  box-shadow: none;
  color: inherit;
  font: inherit;
  /* Leave room for the scrollbar. */
  margin-left: 13px;
  margin-right: 13px;
  margin-top: 5px;
  padding: 0;
  position: fixed;
  right: 0;
  text-align: right;
  top: 0;
  z-index: 10;
}

#login-container:not(.signed-in) {
  cursor: pointer;
}

html[dir='rtl'] #login-container {
  left: 0;
  right: auto;
}

#login-container [is='action-link'] {
  margin-inline-start: 0;
}

.login-status-icon {
  background-position: right center;
  background-repeat: no-repeat;
  min-height: 27px;
  padding-inline-end: 37px;
}

html[dir='rtl'] .login-status-icon {
  background-position-x: left;
}

/* Trash. *********************************************************************/

#trash {
  color: #222;
  height: 100%;
  opacity: 0;
  padding-inline-start: 10px;
  position: absolute;
  right: 0;
  top: 50px;
  transition: top 200ms, opacity 0ms;
  transition-delay: 0ms, 200ms;
  width: auto;
}

html[dir='rtl'] #trash {
  left: 0;
  right: auto;
}

#footer.showing-trash-mode #trash {
  opacity: 0.75;
  top: 0;
  transition-delay: 0ms, 0ms;
  transition-duration: 0ms, 200ms;
}

#footer.showing-trash-mode #trash.drag-target {
  opacity: 1;
}

#trash > .trash-text {
  border: 1px dashed #7f7f7f;
  border-radius: 4px;
  display: inline-block;
  padding-bottom: 9px;
  padding-inline-end: 7px;
  padding-inline-start: 30px;
  padding-top: 10px;
  position: relative;
  top: 7px;
}

#trash > .lid,
#trash > .can {
  left: 18px;
  top: 18px;
}

html[dir='rtl'] #trash > .lid,
html[dir='rtl'] #trash > .can {
  right: 18px;
}

#footer.showing-trash-mode #trash.drag-target .lid {
  transform: rotate(-45deg);
}

html[dir='rtl'] #footer.showing-trash-mode #trash.drag-target .lid {
  transform: rotate(45deg);
}

#fontMeasuringDiv {
  /* The font attributes match the nav inputs. */
  font-size: 0.9em;
  font-weight: bold;
  pointer-events: none;
  position: absolute;
  visibility: hidden;
}

/* Page switcher buttons. *****************************************************/

.page-switcher {
  background-color: transparent;
  border: none;
  bottom: 0;
  font-size: 40px;
  margin: 0;
  max-width: 150px;
  min-width: 90px;
  outline: none;
  padding: 0;
  position: absolute;
  top: 0;
  transition: width 150ms, right 150ms, background-color 150ms;
  z-index: 5;
}

/* Footer buttons. ************************************************************/

#chrome-web-store-link {
  -webkit-order: 3;
  color: rgb(26, 115, 232);
  cursor: pointer;
  display: inline-block;
  margin: 0;
  padding-inline-end: 12px;
  text-decoration: none;
  transition-delay: 100ms;
  white-space: nowrap;
}

#chrome-web-store-title {
  background: -webkit-image-set(url(chrome://theme/IDR_WEBSTORE_ICON_24) 1x, url(chrome://theme/IDR_WEBSTORE_ICON_24@2x) 2x) right 50% no-repeat;
  display: inline-block;
  line-height: 49px;
  padding-inline-end: 36px;
  padding-inline-start: 15px;
}

html[dir='rtl'] #chrome-web-store-title {
  background-position-x: left;
}

/* In trash mode, hide the menus and web store link. */
#footer.showing-trash-mode .menu-container {
  opacity: 0;
  transition-delay: 0ms;
  visibility: hidden;
}

#footer .menu-container {
  -webkit-align-items: center;
  -webkit-flex-direction: row;
  -webkit-justify-content: flex-end;
  /* Put menus in a box so the order can easily be swapped. */
  display: -webkit-flex;
  height: 100%;
  margin: 0;
  min-width: -webkit-min-content;
}

#other-sessions-menu-button {
  -webkit-order: 0;
}

.other-sessions-promo-message {
  display: none;
  padding: 0;
}

.other-sessions-promo-message:only-child {
  display: block;
}

.other-sessions-promo-message p {
  margin: 0;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

.tile-page {
  -webkit-box-orient: vertical;
  display: -webkit-box;
  height: 100%;
  position: relative;
  width: 100%;
}

.tile-page-scrollbar {
  -webkit-box-sizing: border-box;
  margin: 0 4px;
  pointer-events: none;
  position: absolute;
  right: 0;
  width: 5px;
  z-index: 5;
}

.tile-page-content {
  -webkit-box-flex: 1;
  /* Don't apply clip mask to padding. */
  -webkit-mask-clip: content-box;
  /* TODO(estade): this mask is disabled for technical reasons. It negatively
   * impacts performance of page switching, also it causes problems with Mac
   * text: http://crbug.com/86955
  -webkit-mask-image: -webkit-linear-gradient(bottom, transparent, black 30px);
  */
  /* The following four properties are necessary so that the mask won't clip
   * the scrollbar. */
  box-sizing: border-box;
  overflow-y: scroll;
  /* Scrollbar width(13px) + balance right padding.  */
  padding-left: 93px;
  padding-right: 80px;
  /* This value is mirrored in TilePage.updateTopMargin_ */
  padding-top: 60px;
  position: relative;
  text-align: center;
  width: 100%;
}

.top-margin {
  /* The only reason height is set to 1px, rather than left at 0, is that
   * otherwise webkit collapses the top and bottom margins. */
  height: 1px;
}

.tile-grid {
  position: relative;
  width: 100%;
}

.tile {
  -webkit-print-color-adjust: exact;
  /* Don't offer the context menu on long-press. */
  -webkit-touch-callout: none;
  -webkit-user-drag: element;
  display: inline-block;
  position: absolute;
}

/* NOTE: Dopplegangers nest themselves inside of other tiles, so don't
 * accidentally double apply font-size to them. */
.tile:not(.doppleganger) {
  font-size: 1.2em;
}

/* Not real but not a doppleganger: show nothing. This state exists for a
 * webstore tile that's on the same page as a [+]. */
.tile:not(.real):not(.doppleganger) {
  display: none;
}

/* I don't know why this is necessary. -webkit-user-drag: element on .tile
 * should be enough. If we don't do this, we get 2 drag representations for
 * the image. */
.tile img {
  -webkit-user-drag: none;
}

.doppleganger {
  left: 0 !important;
  right: 0 !important;
  top: 0 !important;
}

.tile.dragging {
  opacity: 0;
}

.tile.drag-representation {
  pointer-events: none;
  position: fixed;
  transition: opacity 200ms;
  z-index: 3;
}

.tile.drag-representation.placing > * {
  transition: transform 200ms;
}

/* When a drag finishes while we're not showing the page where the tile
 * belongs, the tile shrinks to a dot. */
.tile.drag-representation.dropped-on-other-page > * {
   transform: scale(0) rotate(0);
}

.tile.drag-representation.deleting > * {
  transform: scale(0) rotate(360deg);
  transition: transform 600ms;
}

.animating-tile-page .tile,
.tile.drag-representation.placing {
  transition: left 200ms, right 200ms, top 200ms;
}

.hovering-on-trash {
  opacity: 0.6;
}

.animating-tile-page .top-margin {
  transition: margin-bottom 200ms;
}

@keyframes bounce {
  0% {
    transform: scale(0, 0);
  }

  60% {
    transform: scale(1.2, 1.2);
  }

  100% {
    transform: scale(1, 1);
  }
}

.tile > .new-tile-contents {
  animation: bounce 500ms ease-in-out;
}

@keyframes blipout {
  0% {
    transform: scale(1, 1);
  }

  60% {
    animation-timing-function: ease-in;
    opacity: 1;
    transform: scale(1.3, 0.02);
  }

  90% {
    opacity: 0.7;
    transform: scale(0.3, 0.02);
  }

  100% {
    animation-timing-function: linear;
    opacity: 0;
    transform: scale(0.3, 0.02);
  }
}

.tile > .removing-tile-contents {
  animation: blipout 300ms;
  animation-fill-mode: forwards;
  pointer-events: none;
}

.tile-page:not(.selected-card) * {
  transition: none !important;
}

/** Scrollbars ****************************************************************/

.tile-page-content::-webkit-scrollbar {
  width: 13px;
}

.tile-page-content::-webkit-scrollbar-button {
  display: none;
}
</style>
<style>/* Copyright (c) 2012 The Chromium Authors. All rights reserved.
 * Use of this source code is governed by a BSD-style license that can be
 * found in the LICENSE file. */

.trash {
  -webkit-appearance: none;
  background: none;
  border: none;
  cursor: pointer;
  display: inline-block;
  outline: none;
  padding: 0;
  position: relative;
  width: 30px;
}

.trash > span {
  display: inline-block;
}

.trash > .can,
.trash > .lid {
  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAQCAQAAACMnYaxAAAAXUlEQVR4XsWQQQrAQAgD84Pti/JSoaftN1MCdgXxXgYvGfUQyABE4DEIUJmeuKgVlJI5em0RGTesFXXZuLwCzvL2pYbHmfCTNSXxpyyajLGClFy7K1dgaaho7YYovIpO3rju6hYFAAAAAElFTkSuQmCC) 0 0 no-repeat;
  left: 8px;
  position: absolute;
  right: 8px;
  top: 2px;
}

.trash > .lid {
  height: 6px;
  transform-origin: -7% 100%;
  transition: transform 150ms;
  width: 14px;
}

html[dir='rtl'] .trash > .lid {
  transform-origin: 107% 100%;
}

.trash:-webkit-any(:focus, :hover, .open) > .lid {
  transform: rotate(-45deg);
  transition: transform 250ms;
}

html[dir='rtl'] .trash:-webkit-any(:focus, :hover, .open) > .lid {
  transform: rotate(45deg);
}

.trash > .can {
  background-position: -1px -4px;
  height: 12px;
  /* The margins match the background position offsets. */
  margin-left: 1px;
  /* The right margin is one greater due to a shadow on the trash image. */
  margin-right: 2px;
  margin-top: 4px;
  width: 11px;
}
</style>
<script>// Copyright 2014 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// Action links are elements that are used to perform an in-page navigation or
// action (e.g. showing a dialog).
//
// They look like normal anchor (<a>) tags as their text color is blue. However,
// they're subtly different as they're not initially underlined (giving users a
// clue that underlined links navigate while action links don't).
//
// Action links look very similar to normal links when hovered (hand cursor,
// underlined). This gives the user an idea that clicking this link will do
// something similar to navigation but in the same page.
//
// They can be created in JavaScript like this (note second arg):
//
//   var link = document.createElement('a', {is: 'action-link'});
//
// or with a constructor like this:
//
//   var link = new ActionLink();
//
// They can be used easily from HTML as well, like so:
//
//   <a is="action-link">Click me!</a>
//
// NOTE: <action-link> and document.createElement('action-link') don't work.

class ActionLink extends HTMLAnchorElement {
  connectedCallback() {
    // Action links can start disabled (e.g. <a is="action-link" disabled>).
    this.tabIndex = this.disabled ? -1 : 0;

    if (!this.hasAttribute('role')) {
      this.setAttribute('role', 'link');
    }

    this.addEventListener('keydown', function(e) {
      if (!this.disabled && e.key === 'Enter' && !this.href) {
        // Schedule a click asynchronously because other 'keydown' handlers
        // may still run later (e.g. document.addEventListener('keydown')).
        // Specifically options dialogs break when this timeout isn't here.
        // NOTE: this affects the "trusted" state of the ensuing click. I
        // haven't found anything that breaks because of this (yet).
        window.setTimeout(this.click.bind(this), 0);
      }
    });

    function preventDefault(e) {
      e.preventDefault();
    }

    function removePreventDefault() {
      document.removeEventListener('selectstart', preventDefault);
      document.removeEventListener('mouseup', removePreventDefault);
    }

    this.addEventListener('mousedown', function() {
      // This handlers strives to match the behavior of <a href="...">.

      // While the mouse is down, prevent text selection from dragging.
      document.addEventListener('selectstart', preventDefault);
      document.addEventListener('mouseup', removePreventDefault);

      // If focus started via mouse press, don't show an outline.
      if (document.activeElement !== this) {
        this.classList.add('no-outline');
      }
    });

    this.addEventListener('blur', function() {
      this.classList.remove('no-outline');
    });
  }

  /** @param {boolean} disabled */
  set disabled(disabled) {
    if (disabled) {
      HTMLAnchorElement.prototype.setAttribute.call(this, 'disabled', '');
    } else {
      HTMLAnchorElement.prototype.removeAttribute.call(this, 'disabled');
    }
    this.tabIndex = disabled ? -1 : 0;
  }

  get disabled() {
    return this.hasAttribute('disabled');
  }

  /** @override */
  setAttribute(attr, val) {
    if (attr.toLowerCase() === 'disabled') {
      this.disabled = true;
    } else {
      HTMLAnchorElement.prototype.setAttribute.apply(this, arguments);
    }
  }

  /** @override */
  removeAttribute(attr) {
    if (attr.toLowerCase() === 'disabled') {
      this.disabled = false;
    } else {
      HTMLAnchorElement.prototype.removeAttribute.apply(this, arguments);
    }
  }
}
customElements.define('action-link', ActionLink, {extends: 'a'});
</script>
<script>// Copyright (c) 2011 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/**
 * @fileoverview EventTracker is a simple class that manages the addition and
 * removal of DOM event listeners. In particular, it keeps track of all
 * listeners that have been added and makes it easy to remove some or all of
 * them without requiring all the information again. This is particularly handy
 * when the listener is a generated function such as a lambda or the result of
 * calling Function.bind.
 */

// eslint-disable-next-line no-var
/* #export */ var EventTracker = class {
  /**
   * Create an EventTracker to track a set of events.
   * EventTracker instances are typically tied 1:1 with other objects or
   * DOM elements whose listeners should be removed when the object is disposed
   * or the corresponding elements are removed from the DOM.
   */
  constructor() {
    /**
     * @type {Array<EventTracker.Entry>}
     * @private
     */
    this.listeners_ = [];
  }

  /**
   * Add an event listener - replacement for EventTarget.addEventListener.
   * @param {!EventTarget} target The DOM target to add a listener to.
   * @param {string} eventType The type of event to subscribe to.
   * @param {EventListener|Function} listener The listener to add.
   * @param {boolean=} opt_capture Whether to invoke during the capture phase.
   */
  add(target, eventType, listener, opt_capture) {
    const capture = !!opt_capture;
    const h = {
      target: target,
      eventType: eventType,
      listener: listener,
      capture: capture,
    };
    this.listeners_.push(h);
    target.addEventListener(eventType, listener, capture);
  }

  /**
   * Remove any specified event listeners added with this EventTracker.
   * @param {!EventTarget} target The DOM target to remove a listener from.
   * @param {?string} eventType The type of event to remove.
   */
  remove(target, eventType) {
    this.listeners_ = this.listeners_.filter(listener => {
      if (listener.target === target &&
          (!eventType || (listener.eventType === eventType))) {
        EventTracker.removeEventListener(listener);
        return false;
      }
      return true;
    });
  }

  /** Remove all event listeners added with this EventTracker. */
  removeAll() {
    this.listeners_.forEach(
        listener => EventTracker.removeEventListener(listener));
    this.listeners_ = [];
  }

  /**
   * Remove a single event listener given it's tracking entry. It's up to the
   * caller to ensure the entry is removed from listeners_.
   * @param {EventTracker.Entry} entry The entry describing the listener to
   * remove.
   */
  static removeEventListener(entry) {
    entry.target.removeEventListener(
        entry.eventType, entry.listener, entry.capture);
  }
};

/**
 * The type of the internal tracking entry.
 * @typedef {{target: !EventTarget,
 *            eventType: string,
 *            listener: (EventListener|Function),
 *            capture: boolean}}
 */
EventTracker.Entry;
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/**
 * Parses a very small subset of HTML. This ensures that insecure HTML /
 * javascript cannot be injected into WebUI.
 * @param {string} s The string to parse.
 * @param {!Array<string>=} opt_extraTags Optional extra allowed tags.
 * @param {!Array<string>=} opt_extraAttrs
 *     Optional extra allowed attributes (all tags are run through these).
 * @throws {Error} In case of non supported markup.
 * @return {DocumentFragment} A document fragment containing the DOM tree.
 */
/* #export */ const parseHtmlSubset = (function() {
  'use strict';

  /** @typedef {function(!Node, string):boolean} */
  let AllowFunction;

  /** @type {!AllowFunction} */
  const allowAttribute = (node, value) => true;

  /**
   * Allow-list of attributes in parseHtmlSubset.
   * @type {!Map<string, !AllowFunction>}
   * @const
   */
  const allowedAttributes = new Map([
    [
      'href',
      (node, value) => {
        // Only allow a[href] starting with chrome:// and https://
        return node.tagName === 'A' &&
            (value.startsWith('chrome://') || value.startsWith('https://'));
      }
    ],
    [
      'target',
      (node, value) => {
        // Only allow a[target='_blank'].
        // TODO(dbeam): are there valid use cases for target !== '_blank'?
        return node.tagName === 'A' && value === '_blank';
      }
    ],
  ]);

  /**
   * Allow-list of optional attributes in parseHtmlSubset.
   * @type {!Map<string, !AllowFunction>}
   * @const
   */
  const allowedOptionalAttributes = new Map([
    ['class', allowAttribute],
    ['id', allowAttribute],
    ['is', (node, value) => value === 'action-link' || value === ''],
    ['role', (node, value) => value === 'link'],
    [
      'src',
      (node, value) => {
        // Only allow img[src] starting with chrome://
        return node.tagName === 'IMG' && value.startsWith('chrome://');
      }
    ],
    ['tabindex', allowAttribute],
  ]);

  /**
   * Allow-list of tag names in parseHtmlSubset.
   * @type {!Set<string>}
   * @const
   */
  const allowedTags =
      new Set(['A', 'B', 'BR', 'DIV', 'P', 'PRE', 'SPAN', 'STRONG']);

  /**
   * Allow-list of optional tag names in parseHtmlSubset.
   * @type {!Set<string>}
   * @const
   */
  const allowedOptionalTags = new Set(['IMG']);

  /**
   * This policy maps a given string to a `TrustedHTML` object
   * without performing any validation. Callsites must ensure
   * that the resulting object will only be used in inert
   * documents. Initialized lazily.
   * @type {!TrustedTypePolicy}
   */
  let unsanitizedPolicy;

  /**
   * @param {!Array<string>} optTags an Array to merge.
   * @return {!Set<string>} Set of allowed tags.
   */
  function mergeTags(optTags) {
    const clone = new Set(allowedTags);
    optTags.forEach(str => {
      const tag = str.toUpperCase();
      if (allowedOptionalTags.has(tag)) {
        clone.add(tag);
      }
    });
    return clone;
  }

  /**
   * @param {!Array<string>} optAttrs an Array to merge.
   * @return {!Map<string, !AllowFunction>} Map of allowed
   *     attributes.
   */
  function mergeAttrs(optAttrs) {
    const clone = new Map([...allowedAttributes]);
    optAttrs.forEach(key => {
      if (allowedOptionalAttributes.has(key)) {
        clone.set(key, allowedOptionalAttributes.get(key));
      }
    });
    return clone;
  }

  function walk(n, f) {
    f(n);
    for (let i = 0; i < n.childNodes.length; i++) {
      walk(n.childNodes[i], f);
    }
  }

  function assertElement(tags, node) {
    if (!tags.has(node.tagName)) {
      throw Error(node.tagName + ' is not supported');
    }
  }

  function assertAttribute(attrs, attrNode, node) {
    const n = attrNode.nodeName;
    const v = attrNode.nodeValue;
    if (!attrs.has(n) || !attrs.get(n)(node, v)) {
      throw Error(node.tagName + '[' + n + '="' + v + '"] is not supported');
    }
  }

  return function(s, opt_extraTags, opt_extraAttrs) {
    const tags = opt_extraTags ? mergeTags(opt_extraTags) : allowedTags;
    const attrs =
        opt_extraAttrs ? mergeAttrs(opt_extraAttrs) : allowedAttributes;

    const doc = document.implementation.createHTMLDocument('');
    const r = doc.createRange();
    r.selectNode(doc.body);

    if (window.trustedTypes) {
      if (!unsanitizedPolicy) {
        unsanitizedPolicy = trustedTypes.createPolicy(
            'parse-html-subset', {createHTML: untrustedHTML => untrustedHTML});
      }
      s = unsanitizedPolicy.createHTML(s);
    }

    // This does not execute any scripts because the document has no view.
    const df = r.createContextualFragment(s);
    walk(df, function(node) {
      switch (node.nodeType) {
        case Node.ELEMENT_NODE:
          assertElement(tags, node);
          const nodeAttrs = node.attributes;
          for (let i = 0; i < nodeAttrs.length; ++i) {
            assertAttribute(attrs, nodeAttrs[i], node);
          }
          break;

        case Node.COMMENT_NODE:
        case Node.DOCUMENT_FRAGMENT_NODE:
        case Node.TEXT_NODE:
          break;

        default:
          throw Error('Node type ' + node.nodeType + ' is not supported');
      }
    });
    return df;
  };
})();
</script>

<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/** @typedef {{eventName: string, uid: number}} */
// eslint-disable-next-line no-var
var WebUIListener;

/** Platform, package, object property, and Event support. **/
// eslint-disable-next-line no-var
var cr = cr || function(global) {
  'use strict';

  /**
   * Builds an object structure for the provided namespace path,
   * ensuring that names that already exist are not overwritten. For
   * example:
   * "a.b.c" -> a = {};a.b={};a.b.c={};
   * @param {string} name Name of the object that this file defines.
   * @return {!Object} The last object exported (i.e. exportPath('cr.ui')
   *     returns a reference to the ui property of window.cr).
   * @private
   */
  function exportPath(name) {
    const parts = name.split('.');
    let cur = global;

    for (let part; parts.length && (part = parts.shift());) {
      if (part in cur) {
        cur = cur[part];
      } else {
        cur = cur[part] = {};
      }
    }
    return cur;
  }

  /**
   * Fires a property change event on the target.
   * @param {EventTarget} target The target to dispatch the event on.
   * @param {string} propertyName The name of the property that changed.
   * @param {*} newValue The new value for the property.
   * @param {*} oldValue The old value for the property.
   */
  function dispatchPropertyChange(target, propertyName, newValue, oldValue) {
    const e = new Event(propertyName + 'Change');
    e.propertyName = propertyName;
    e.newValue = newValue;
    e.oldValue = oldValue;
    target.dispatchEvent(e);
  }

  /**
   * Converts a camelCase javascript property name to a hyphenated-lower-case
   * attribute name.
   * @param {string} jsName The javascript camelCase property name.
   * @return {string} The equivalent hyphenated-lower-case attribute name.
   */
  function getAttributeName(jsName) {
    return jsName.replace(/([A-Z])/g, '-$1').toLowerCase();
  }

  /**
   * The kind of property to define in {@code defineProperty}.
   * @enum {string}
   * @const
   */
  const PropertyKind = {
    /**
     * Plain old JS property where the backing data is stored as a "private"
     * field on the object.
     * Use for properties of any type. Type will not be checked.
     */
    JS: 'js',

    /**
     * The property backing data is stored as an attribute on an element.
     * Use only for properties of type {string}.
     */
    ATTR: 'attr',

    /**
     * The property backing data is stored as an attribute on an element. If the
     * element has the attribute then the value is true.
     * Use only for properties of type {boolean}.
     */
    BOOL_ATTR: 'boolAttr'
  };

  /**
   * Helper function for defineProperty that returns the getter to use for the
   * property.
   * @param {string} name The name of the property.
   * @param {PropertyKind} kind The kind of the property.
   * @return {function():*} The getter for the property.
   */
  function getGetter(name, kind) {
    let attributeName;
    switch (kind) {
      case PropertyKind.JS:
        const privateName = name + '_';
        return function() {
          return this[privateName];
        };
      case PropertyKind.ATTR:
        attributeName = getAttributeName(name);
        return function() {
          return this.getAttribute(attributeName);
        };
      case PropertyKind.BOOL_ATTR:
        attributeName = getAttributeName(name);
        return function() {
          return this.hasAttribute(attributeName);
        };
    }

    // TODO(dbeam): replace with assertNotReached() in assert.js when I can coax
    // the browser/unit tests to preprocess this file through grit.
    throw 'not reached';
  }

  /**
   * Helper function for defineProperty that returns the setter of the right
   * kind.
   * @param {string} name The name of the property we are defining the setter
   *     for.
   * @param {PropertyKind} kind The kind of property we are getting the
   *     setter for.
   * @param {function(*, *):void=} opt_setHook A function to run after the
   *     property is set, but before the propertyChange event is fired.
   * @return {function(*):void} The function to use as a setter.
   */
  function getSetter(name, kind, opt_setHook) {
    let attributeName;
    switch (kind) {
      case PropertyKind.JS:
        const privateName = name + '_';
        return function(value) {
          const oldValue = this[name];
          if (value !== oldValue) {
            this[privateName] = value;
            if (opt_setHook) {
              opt_setHook.call(this, value, oldValue);
            }
            dispatchPropertyChange(this, name, value, oldValue);
          }
        };

      case PropertyKind.ATTR:
        attributeName = getAttributeName(name);
        return function(value) {
          const oldValue = this[name];
          if (value !== oldValue) {
            if (value === undefined) {
              this.removeAttribute(attributeName);
            } else {
              this.setAttribute(attributeName, value);
            }
            if (opt_setHook) {
              opt_setHook.call(this, value, oldValue);
            }
            dispatchPropertyChange(this, name, value, oldValue);
          }
        };

      case PropertyKind.BOOL_ATTR:
        attributeName = getAttributeName(name);
        return function(value) {
          const oldValue = this[name];
          if (value !== oldValue) {
            if (value) {
              this.setAttribute(attributeName, name);
            } else {
              this.removeAttribute(attributeName);
            }
            if (opt_setHook) {
              opt_setHook.call(this, value, oldValue);
            }
            dispatchPropertyChange(this, name, value, oldValue);
          }
        };
    }

    // TODO(dbeam): replace with assertNotReached() in assert.js when I can coax
    // the browser/unit tests to preprocess this file through grit.
    throw 'not reached';
  }

  /**
   * Defines a property on an object. When the setter changes the value a
   * property change event with the type {@code name + 'Change'} is fired.
   * @param {!Object} obj The object to define the property for.
   * @param {string} name The name of the property.
   * @param {PropertyKind=} opt_kind What kind of underlying storage to use.
   * @param {function(*, *):void=} opt_setHook A function to run after the
   *     property is set, but before the propertyChange event is fired.
   *
   * TODO(crbug.com/425829): This function makes use of deprecated getter or
   * setter functions.
   * @suppress {deprecated}
   */
  function defineProperty(obj, name, opt_kind, opt_setHook) {
    if (typeof obj === 'function') {
      obj = obj.prototype;
    }

    const kind = /** @type {PropertyKind} */ (opt_kind || PropertyKind.JS);

    // TODO(crbug.com/425829): Remove above suppression once we no longer use
    // deprecated functions lookupGetter, defineGetter, lookupSetter, and
    // defineSetter.
    // eslint-disable-next-line no-restricted-properties
    if (!obj.__lookupGetter__(name)) {
      // eslint-disable-next-line no-restricted-properties
      obj.__defineGetter__(name, getGetter(name, kind));
    }

    // eslint-disable-next-line no-restricted-properties
    if (!obj.__lookupSetter__(name)) {
      // eslint-disable-next-line no-restricted-properties
      obj.__defineSetter__(name, getSetter(name, kind, opt_setHook));
    }
  }

  /**
   * Returns a getter and setter to be used as property descriptor in
   * Object.defineProperty(). When the setter changes the value a property
   * change event with the type {@code name + 'Change'} is fired.
   * @param {string} name The name of the property.
   * @param {PropertyKind=} opt_kind What kind of underlying storage to use.
   * @param {function(*, *):void=} opt_setHook A function to run after the
   *     property is set, but before the propertyChange event is fired.
   */
  function getPropertyDescriptor(name, opt_kind, opt_setHook) {
    const kind = /** @type {PropertyKind} */ (opt_kind || PropertyKind.JS);

    const desc = {
      get: getGetter(name, kind),
      set: getSetter(name, kind, opt_setHook),
    };
    return desc;
  }

  /**
   * Counter for use with createUid
   */
  let uidCounter = 1;

  /**
   * @return {number} A new unique ID.
   */
  function createUid() {
    return uidCounter++;
  }

  /**
   * Dispatches a simple event on an event target.
   * @param {!EventTarget} target The event target to dispatch the event on.
   * @param {string} type The type of the event.
   * @param {boolean=} opt_bubbles Whether the event bubbles or not.
   * @param {boolean=} opt_cancelable Whether the default action of the event
   *     can be prevented. Default is true.
   * @return {boolean} If any of the listeners called {@code preventDefault}
   *     during the dispatch this will return false.
   */
  function dispatchSimpleEvent(target, type, opt_bubbles, opt_cancelable) {
    const e = new Event(type, {
      bubbles: opt_bubbles,
      cancelable: opt_cancelable === undefined || opt_cancelable
    });
    return target.dispatchEvent(e);
  }

  /**
   * Calls |fun| and adds all the fields of the returned object to the object
   * named by |name|. For example, cr.define('cr.ui', function() {
   *   function List() {
   *     ...
   *   }
   *   function ListItem() {
   *     ...
   *   }
   *   return {
   *     List: List,
   *     ListItem: ListItem,
   *   };
   * });
   * defines the functions cr.ui.List and cr.ui.ListItem.
   * @param {string} name The name of the object that we are adding fields to.
   * @param {!Function} fun The function that will return an object containing
   *     the names and values of the new fields.
   */
  function define(name, fun) {
    const obj = exportPath(name);
    const exports = fun();
    for (const propertyName in exports) {
      // Maybe we should check the prototype chain here? The current usage
      // pattern is always using an object literal so we only care about own
      // properties.
      const propertyDescriptor =
          Object.getOwnPropertyDescriptor(exports, propertyName);
      if (propertyDescriptor) {
        Object.defineProperty(obj, propertyName, propertyDescriptor);
      }
    }
  }

  /**
   * Adds a {@code getInstance} static method that always return the same
   * instance object.
   * @param {!Function} ctor The constructor for the class to add the static
   *     method to.
   */
  function addSingletonGetter(ctor) {
    ctor.getInstance = function() {
      return ctor.instance_ || (ctor.instance_ = new ctor());
    };
  }

  /**
   * The mapping used by the sendWithPromise mechanism to tie the Promise
   * returned to callers with the corresponding WebUI response. The mapping is
   * from ID to the PromiseResolver helper; the ID is generated by
   * sendWithPromise and is unique across all invocations of said method.
   * @type {!Object<!PromiseResolver>}
   */
  const chromeSendResolverMap = {};

  /**
   * The named method the WebUI handler calls directly in response to a
   * chrome.send call that expects a response. The handler requires no knowledge
   * of the specific name of this method, as the name is passed to the handler
   * as the first argument in the arguments list of chrome.send. The handler
   * must pass the ID, also sent via the chrome.send arguments list, as the
   * first argument of the JS invocation; additionally, the handler may
   * supply any number of other arguments that will be included in the response.
   * @param {string} id The unique ID identifying the Promise this response is
   *     tied to.
   * @param {boolean} isSuccess Whether the request was successful.
   * @param {*} response The response as sent from C++.
   */
  function webUIResponse(id, isSuccess, response) {
    const resolver = chromeSendResolverMap[id];
    delete chromeSendResolverMap[id];

    if (isSuccess) {
      resolver.resolve(response);
    } else {
      resolver.reject(response);
    }
  }

  /**
   * A variation of chrome.send, suitable for messages that expect a single
   * response from C++.
   * @param {string} methodName The name of the WebUI handler API.
   * @param {...*} var_args Variable number of arguments to be forwarded to the
   *     C++ call.
   * @return {!Promise}
   */
  function sendWithPromise(methodName, var_args) {
    const args = Array.prototype.slice.call(arguments, 1);
    const promiseResolver = new PromiseResolver();
    const id = methodName + '_' + createUid();
    chromeSendResolverMap[id] = promiseResolver;
    chrome.send(methodName, [id].concat(args));
    return promiseResolver.promise;
  }

  /**
   * A map of maps associating event names with listeners. The 2nd level map
   * associates a listener ID with the callback function, such that individual
   * listeners can be removed from an event without affecting other listeners of
   * the same event.
   * @type {!Object<!Object<!Function>>}
   */
  const webUIListenerMap = {};

  /**
   * The named method the WebUI handler calls directly when an event occurs.
   * The WebUI handler must supply the name of the event as the first argument
   * of the JS invocation; additionally, the handler may supply any number of
   * other arguments that will be forwarded to the listener callbacks.
   * @param {string} event The name of the event that has occurred.
   * @param {...*} var_args Additional arguments passed from C++.
   */
  function webUIListenerCallback(event, var_args) {
    const eventListenersMap = webUIListenerMap[event];
    if (!eventListenersMap) {
      // C++ event sent for an event that has no listeners.
      // TODO(dpapad): Should a warning be displayed here?
      return;
    }

    const args = Array.prototype.slice.call(arguments, 1);
    for (const listenerId in eventListenersMap) {
      eventListenersMap[listenerId].apply(null, args);
    }
  }

  /**
   * Registers a listener for an event fired from WebUI handlers. Any number of
   * listeners may register for a single event.
   * @param {string} eventName The event to listen to.
   * @param {!Function} callback The callback run when the event is fired.
   * @return {!WebUIListener} An object to be used for removing a listener via
   *     cr.removeWebUIListener. Should be treated as read-only.
   */
  function addWebUIListener(eventName, callback) {
    webUIListenerMap[eventName] = webUIListenerMap[eventName] || {};
    const uid = createUid();
    webUIListenerMap[eventName][uid] = callback;
    return {eventName: eventName, uid: uid};
  }

  /**
   * Removes a listener. Does nothing if the specified listener is not found.
   * @param {!WebUIListener} listener The listener to be removed (as returned by
   *     addWebUIListener).
   * @return {boolean} Whether the given listener was found and actually
   *     removed.
   */
  function removeWebUIListener(listener) {
    const listenerExists = webUIListenerMap[listener.eventName] &&
        webUIListenerMap[listener.eventName][listener.uid];
    if (listenerExists) {
      delete webUIListenerMap[listener.eventName][listener.uid];
      return true;
    }
    return false;
  }

  return {
    addSingletonGetter: addSingletonGetter,
    define: define,
    defineProperty: defineProperty,
    getPropertyDescriptor: getPropertyDescriptor,
    dispatchPropertyChange: dispatchPropertyChange,
    dispatchSimpleEvent: dispatchSimpleEvent,
    PropertyKind: PropertyKind,

    // C++ <-> JS communication related methods.
    addWebUIListener: addWebUIListener,
    removeWebUIListener: removeWebUIListener,
    sendWithPromise: sendWithPromise,
    webUIListenerCallback: webUIListenerCallback,
    webUIResponse: webUIResponse,

    /** Whether we are using a Mac or not. */
    get isMac() {
      return /Mac/.test(navigator.platform);
    },

    /** Whether this is on the Windows platform or not. */
    get isWindows() {
      return /Win/.test(navigator.platform);
    },

    /** Whether this is the ChromeOS/ash web browser. */
    get isChromeOS() {
      let returnValue = false;
      // TODO(https://crbug.com/1118190): grit conditionals do not work in many
      // WebUI tests.
      // 
      return returnValue;
    },

    /** Whether this is the ChromeOS/Lacros web browser. */
    get isLacros() {
      let returnValue = false;
      // TODO(https://crbug.com/1118190): grit conditionals do not work in many
      // WebUI tests.
      // 
      return returnValue;
    },

    /** Whether this is on vanilla Linux (not chromeOS). */
    get isLinux() {
      return /Linux/.test(navigator.userAgent);
    },

    /** Whether this is on Android. */
    get isAndroid() {
      return /Android/.test(navigator.userAgent);
    },

    /** Whether this is on iOS. */
    get isIOS() {
      return /CriOS/.test(navigator.userAgent);
    }
  };
}(this);
</script>
<script>// Copyright (c) 2010 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/**
 * @fileoverview Work-around for
 * https://github.com/google/closure-compiler/issues/3143, such that WebUI code
 * can use the native EventTarget class.
 * TODO(dpapad): Remove this entire file if/when that issue is fixed.
 */

cr.define('cr', function() {
  /**
   * @constructor
   * @implements {EventTarget}
   */
  /* #export */ const NativeEventTarget = self['EventTarget'];

  /** @override */ NativeEventTarget.prototype.addEventListener;
  /** @override */ NativeEventTarget.prototype.dispatchEvent;
  /** @override */ NativeEventTarget.prototype.removeEventListener;

  // #cr_define_end
  return {EventTarget: NativeEventTarget};
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

cr.define('cr.ui', function() {
  /**
   * Decorates elements as an instance of a class.
   * @param {string|!Element} source The way to find the element(s) to decorate.
   *     If this is a string then {@code querySeletorAll} is used to find the
   *     elements to decorate.
   * @param {!Function} constr The constructor to decorate with. The constr
   *     needs to have a {@code decorate} function.
   * @closurePrimitive {asserts.matchesReturn}
   */
  /* #export */ function decorate(source, constr) {
    let elements;
    if (typeof source === 'string') {
      elements = document.querySelectorAll(source);
    } else {
      elements = [source];
    }

    for (let i = 0, el; el = elements[i]; i++) {
      if (!(el instanceof constr)) {
        constr.decorate(el);
      }
    }
  }

  /**
   * Helper function for creating new element for define.
   */
  function createElementHelper(tagName, opt_bag) {
    // Allow passing in ownerDocument to create in a different document.
    let doc;
    if (opt_bag && opt_bag.ownerDocument) {
      doc = opt_bag.ownerDocument;
    } else {
      doc = document;
    }
    return doc.createElement(tagName);
  }

  /**
   * Creates the constructor for a UI element class.
   *
   * Usage:
   * <pre>
   * var List = cr.ui.define('list');
   * List.prototype = {
   *   __proto__: HTMLUListElement.prototype,
   *   decorate() {
   *     ...
   *   },
   *   ...
   * };
   * </pre>
   *
   * @param {string|Function} tagNameOrFunction The tagName or
   *     function to use for newly created elements. If this is a function it
   *     needs to return a new element when called.
   * @return {function(Object=):Element} The constructor function which takes
   *     an optional property bag. The function also has a static
   *     {@code decorate} method added to it.
   */
  /* #export */ function define(tagNameOrFunction) {
    let createFunction, tagName;
    if (typeof tagNameOrFunction === 'function') {
      createFunction = tagNameOrFunction;
      tagName = '';
    } else {
      createFunction = createElementHelper;
      tagName = tagNameOrFunction;
    }

    /**
     * Creates a new UI element constructor.
     * @param {Object=} opt_propertyBag Optional bag of properties to set on the
     *     object after created. The property {@code ownerDocument} is special
     *     cased and it allows you to create the element in a different
     *     document than the default.
     * @constructor
     */
    function f(opt_propertyBag) {
      const el = createFunction(tagName, opt_propertyBag);
      f.decorate(el);
      for (const propertyName in opt_propertyBag) {
        el[propertyName] = opt_propertyBag[propertyName];
      }
      return el;
    }

    /**
     * Decorates an element as a UI element class.
     * @param {!Element} el The element to decorate.
     */
    f.decorate = function(el) {
      el.__proto__ = f.prototype;
      if (el.decorate) {
        el.decorate();
      }
    };

    return f;
  }

  /**
   * Input elements do not grow and shrink with their content. This is a simple
   * (and not very efficient) way of handling shrinking to content with support
   * for min width and limited by the width of the parent element.
   * @param {!HTMLElement} el The element to limit the width for.
   * @param {!HTMLElement} parentEl The parent element that should limit the
   *     size.
   * @param {number} min The minimum width.
   * @param {number=} opt_scale Optional scale factor to apply to the width.
   */
  /* #export */ function limitInputWidth(el, parentEl, min, opt_scale) {
    // Needs a size larger than borders
    el.style.width = '10px';
    const doc = el.ownerDocument;
    const win = doc.defaultView;
    const computedStyle = win.getComputedStyle(el);
    const parentComputedStyle = win.getComputedStyle(parentEl);
    const rtl = computedStyle.direction === 'rtl';

    // To get the max width we get the width of the treeItem minus the position
    // of the input.
    const inputRect = el.getBoundingClientRect();  // box-sizing
    const parentRect = parentEl.getBoundingClientRect();
    const startPos = rtl ? parentRect.right - inputRect.right :
                           inputRect.left - parentRect.left;

    // Add up border and padding of the input.
    const inner = parseInt(computedStyle.borderLeftWidth, 10) +
        parseInt(computedStyle.paddingLeft, 10) +
        parseInt(computedStyle.paddingRight, 10) +
        parseInt(computedStyle.borderRightWidth, 10);

    // We also need to subtract the padding of parent to prevent it to overflow.
    const parentPadding = rtl ? parseInt(parentComputedStyle.paddingLeft, 10) :
                                parseInt(parentComputedStyle.paddingRight, 10);

    let max = parentEl.clientWidth - startPos - inner - parentPadding;
    if (opt_scale) {
      max *= opt_scale;
    }

    function limit() {
      if (el.scrollWidth > max) {
        el.style.width = max + 'px';
      } else {
        el.style.width = 0;
        const sw = el.scrollWidth;
        if (sw < min) {
          el.style.width = min + 'px';
        } else {
          el.style.width = sw + 'px';
        }
      }
    }

    el.addEventListener('input', limit);
    limit();
  }

  /**
   * Takes a number and spits out a value CSS will be happy with. To avoid
   * subpixel layout issues, the value is rounded to the nearest integral value.
   * @param {number} pixels The number of pixels.
   * @return {string} e.g. '16px'.
   */
  /* #export */ function toCssPx(pixels) {
    if (!window.isFinite(pixels)) {
      console.error('Pixel value is not a number: ' + pixels);
    }
    return Math.round(pixels) + 'px';
  }

  /**
   * Users complain they occasionaly use doubleclicks instead of clicks
   * (http://crbug.com/140364). To fix it we freeze click handling for
   * the doubleclick time interval.
   * @param {MouseEvent} e Initial click event.
   */
  /* #export */ function swallowDoubleClick(e) {
    const doc = e.target.ownerDocument;
    let counter = Math.min(1, e.detail);
    function swallow(e) {
      e.stopPropagation();
      e.preventDefault();
    }
    function onclick(e) {
      if (e.detail > counter) {
        counter = e.detail;
        // Swallow the click since it's a click inside the doubleclick timeout.
        swallow(e);
      } else {
        // Stop tracking clicks and let regular handling.
        doc.removeEventListener('dblclick', swallow, true);
        doc.removeEventListener('click', onclick, true);
      }
    }
    // The following 'click' event (if e.type === 'mouseup') mustn't be taken
    // into account (it mustn't stop tracking clicks). Start event listening
    // after zero timeout.
    setTimeout(function() {
      doc.addEventListener('click', onclick, true);
      doc.addEventListener('dblclick', swallow, true);
    }, 0);
  }

  // #cr_define_end
  return {
    decorate: decorate,
    define: define,
    limitInputWidth: limitInputWidth,
    toCssPx: toCssPx,
    swallowDoubleClick: swallowDoubleClick
  };
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// require: event_tracker.js

cr.define('cr.ui', function() {
  /**
   * The arrow location specifies how the arrow and bubble are positioned in
   * relation to the anchor node.
   * @enum {string}
   */
  const ArrowLocation = {
    // The arrow is positioned at the top and the start of the bubble. In left
    // to right mode this is the top left. The entire bubble is positioned below
    // the anchor node.
    TOP_START: 'top-start',
    // The arrow is positioned at the top and the end of the bubble. In left to
    // right mode this is the top right. The entire bubble is positioned below
    // the anchor node.
    TOP_END: 'top-end',
    // The arrow is positioned at the bottom and the start of the bubble. In
    // left to right mode this is the bottom left. The entire bubble is
    // positioned above the anchor node.
    BOTTOM_START: 'bottom-start',
    // The arrow is positioned at the bottom and the end of the bubble. In
    // left to right mode this is the bottom right. The entire bubble is
    // positioned above the anchor node.
    BOTTOM_END: 'bottom-end'
  };

  /**
   * The bubble alignment specifies the position of the bubble in relation to
   * the anchor node.
   * @enum {string}
   */
  const BubbleAlignment = {
    // The bubble is positioned just above or below the anchor node (as
    // specified by the arrow location) so that the arrow points at the midpoint
    // of the anchor.
    ARROW_TO_MID_ANCHOR: 'arrow-to-mid-anchor',
    // The bubble is positioned just above or below the anchor node (as
    // specified by the arrow location) so that its reference edge lines up with
    // the edge of the anchor.
    BUBBLE_EDGE_TO_ANCHOR_EDGE: 'bubble-edge-anchor-edge',
    // The bubble is positioned so that it is entirely within view and does not
    // obstruct the anchor element, if possible. The specified arrow location is
    // taken into account as the preferred alignment but may be overruled if
    // there is insufficient space (see BubbleBase.reposition for the exact
    // placement algorithm).
    ENTIRELY_VISIBLE: 'entirely-visible'
  };

  /**
   * Abstract base class that provides common functionality for implementing
   * free-floating informational bubbles with a triangular arrow pointing at an
   * anchor node.
   * @constructor
   * @extends {HTMLDivElement}
   * @implements {EventListener}
   */
  const BubbleBase = cr.ui.define('div');

  /**
   * The horizontal distance between the tip of the arrow and the reference edge
   * of the bubble (as specified by the arrow location). In pixels.
   * @type {number}
   * @const
   */
  BubbleBase.ARROW_OFFSET = 30;

  /**
   * Minimum horizontal spacing between edge of bubble and edge of viewport
   * (when using the ENTIRELY_VISIBLE alignment). In pixels.
   * @type {number}
   * @const
   */
  BubbleBase.MIN_VIEWPORT_EDGE_MARGIN = 2;

  /**
   * This is used to create TrustedHTML.
   * @type {!TrustedTypePolicy}
   */
  const staticHtmlPolicy = trustedTypes.createPolicy('cr-ui-bubble-js-static', {
    createHTML: () => {
      return '<div class="bubble-content"></div>' +
          '<div class="bubble-shadow"></div>' +
          '<div class="bubble-arrow"></div>';
    },
  });

  BubbleBase.prototype = {
    // Set up the prototype chain.
    __proto__: HTMLDivElement.prototype,

    /**
     * @type {Node}
     * @private
     */
    anchorNode_: null,

    /**
     * Initialization function for the cr.ui framework.
     */
    decorate() {
      this.className = 'bubble';
      // TODO(Jun.Kokatsu@microsoft.com): remove an empty string argument
      // once supported.
      // https://github.com/w3c/webappsec-trusted-types/issues/278
      this.innerHTML = staticHtmlPolicy.createHTML('');
      this.hidden = true;
      this.bubbleAlignment = cr.ui.BubbleAlignment.ENTIRELY_VISIBLE;
    },

    /**
     * Set the anchor node, i.e. the node that this bubble points at. Only
     * available when the bubble is not being shown.
     * @param {HTMLElement} node The new anchor node.
     */
    set anchorNode(node) {
      if (!this.hidden) {
        return;
      }

      this.anchorNode_ = node;
    },

    /**
     * Set the conent of the bubble. Only available when the bubble is not being
     * shown.
     * @param {HTMLElement} node The root node of the new content.
     */
    set content(node) {
      if (!this.hidden) {
        return;
      }

      const bubbleContent = this.querySelector('.bubble-content');
      bubbleContent.innerHTML = trustedTypes.emptyHTML;
      bubbleContent.appendChild(node);
    },

    /**
     * Set the arrow location. Only available when the bubble is not being
     * shown.
     * @param {cr.ui.ArrowLocation} location The new arrow location.
     */
    set arrowLocation(location) {
      if (!this.hidden) {
        return;
      }

      this.arrowAtRight_ = location === cr.ui.ArrowLocation.TOP_END ||
          location === cr.ui.ArrowLocation.BOTTOM_END;
      if (document.documentElement.dir === 'rtl') {
        this.arrowAtRight_ = !this.arrowAtRight_;
      }
      this.arrowAtTop_ = location === cr.ui.ArrowLocation.TOP_START ||
          location === cr.ui.ArrowLocation.TOP_END;
    },

    /**
     * Set the bubble alignment. Only available when the bubble is not being
     * shown.
     * @param {cr.ui.BubbleAlignment} alignment The new bubble alignment.
     */
    set bubbleAlignment(alignment) {
      if (!this.hidden) {
        return;
      }

      this.bubbleAlignment_ = alignment;
    },

    /**
     * Update the position of the bubble. Whenever the layout may have changed,
     * the bubble should either be repositioned by calling this function or
     * hidden so that it does not point to a nonsensical location on the page.
     */
    reposition() {
      const documentWidth = document.documentElement.clientWidth;
      const documentHeight = document.documentElement.clientHeight;
      const anchor = this.anchorNode_.getBoundingClientRect();
      const anchorMid = (anchor.left + anchor.right) / 2;
      const bubble = this.getBoundingClientRect();
      const arrow = this.querySelector('.bubble-arrow').getBoundingClientRect();

      let left;
      let top;
      if (this.bubbleAlignment_ === cr.ui.BubbleAlignment.ENTIRELY_VISIBLE) {
        // Work out horizontal placement. The bubble is initially positioned so
        // that the arrow tip points toward the midpoint of the anchor and is
        // BubbleBase.ARROW_OFFSET pixels from the reference edge and (as
        // specified by the arrow location). If the bubble is not entirely
        // within view, it is then shifted, preserving the arrow tip position.
        left = this.arrowAtRight_ ?
            anchorMid + BubbleBase.ARROW_OFFSET - bubble.width :
            anchorMid - BubbleBase.ARROW_OFFSET;
        const maxLeftPos =
            documentWidth - bubble.width - BubbleBase.MIN_VIEWPORT_EDGE_MARGIN;
        const minLeftPos = BubbleBase.MIN_VIEWPORT_EDGE_MARGIN;
        if (document.documentElement.dir === 'rtl') {
          left = Math.min(Math.max(left, minLeftPos), maxLeftPos);
        } else {
          left = Math.max(Math.min(left, maxLeftPos), minLeftPos);
        }
        const arrowTip = Math.min(
            Math.max(
                arrow.width / 2,
                this.arrowAtRight_ ? left + bubble.width - anchorMid :
                                     anchorMid - left),
            bubble.width - arrow.width / 2);

        // Work out the vertical placement, attempting to fit the bubble
        // entirely into view. The following placements are considered in
        // decreasing order of preference:
        // * Outside the anchor, arrow tip touching the anchor (arrow at
        //   top/bottom as specified by the arrow location).
        // * Outside the anchor, arrow tip touching the anchor (arrow at
        //   bottom/top, opposite the specified arrow location).
        // * Outside the anchor, arrow tip overlapping the anchor (arrow at
        //   top/bottom as specified by the arrow location).
        // * Outside the anchor, arrow tip overlapping the anchor (arrow at
        //   bottom/top, opposite the specified arrow location).
        // * Overlapping the anchor.
        const offsetTop = Math.min(
            documentHeight - anchor.bottom - bubble.height, arrow.height / 2);
        const offsetBottom =
            Math.min(anchor.top - bubble.height, arrow.height / 2);
        if (offsetTop < 0 && offsetBottom < 0) {
          top = 0;
          this.updateArrowPosition_(false, false, arrowTip);
        } else if (
            offsetTop > offsetBottom ||
            offsetTop === offsetBottom && this.arrowAtTop_) {
          top = anchor.bottom + offsetTop;
          this.updateArrowPosition_(true, true, arrowTip);
        } else {
          top = anchor.top - bubble.height - offsetBottom;
          this.updateArrowPosition_(true, false, arrowTip);
        }
      } else {
        if (this.bubbleAlignment_ ===
            cr.ui.BubbleAlignment.BUBBLE_EDGE_TO_ANCHOR_EDGE) {
          left = this.arrowAtRight_ ? anchor.right - bubble.width : anchor.left;
        } else {
          left = this.arrowAtRight_ ?
              anchorMid - this.clientWidth + BubbleBase.ARROW_OFFSET :
              anchorMid - BubbleBase.ARROW_OFFSET;
        }
        top = this.arrowAtTop_ ?
            anchor.bottom + arrow.height / 2 :
            anchor.top - this.clientHeight - arrow.height / 2;
        this.updateArrowPosition_(
            true, this.arrowAtTop_, BubbleBase.ARROW_OFFSET);
      }

      this.style.left = left + 'px';
      this.style.top = top + 'px';
    },

    /**
     * Show the bubble.
     */
    show() {
      if (!this.hidden) {
        return;
      }

      this.attachToDOM_();
      this.hidden = false;
      this.reposition();

      const doc = assert(this.ownerDocument);
      this.eventTracker_ = new EventTracker;
      this.eventTracker_.add(doc, 'keydown', this, true);
      this.eventTracker_.add(doc, 'mousedown', this, true);
    },

    /**
     * Hide the bubble.
     */
    hide() {
      if (this.hidden) {
        return;
      }

      this.eventTracker_.removeAll();
      this.hidden = true;
      this.parentNode.removeChild(this);
    },

    /**
     * Handle keyboard events, dismissing the bubble if necessary.
     * @param {Event} event The event.
     */
    handleEvent(event) {
      // Close the bubble when the user presses <Esc>.
      if (event.type === 'keydown' && event.keyCode === 27) {
        this.hide();
        event.preventDefault();
        event.stopPropagation();
      }
    },

    /**
     * Attach the bubble to the document's DOM.
     * @private
     */
    attachToDOM_() {
      document.body.appendChild(this);
    },

    /**
     * Update the arrow so that it appears at the correct position.
     * @param {boolean} visible Whether the arrow should be visible.
     * @param {boolean} atTop Whether the arrow should be at the top of the
     * bubble.
     * @param {number} tipOffset The horizontal distance between the tip of the
     * arrow and the reference edge of the bubble (as specified by the arrow
     * location).
     * @private
     */
    updateArrowPosition_(visible, atTop, tipOffset) {
      const bubbleArrow = this.querySelector('.bubble-arrow');
      bubbleArrow.hidden = !visible;
      if (!visible) {
        return;
      }

      let edgeOffset = (-bubbleArrow.clientHeight / 2) + 'px';
      bubbleArrow.style.top = atTop ? edgeOffset : 'auto';
      bubbleArrow.style.bottom = atTop ? 'auto' : edgeOffset;

      edgeOffset = (tipOffset - bubbleArrow.offsetWidth / 2) + 'px';
      bubbleArrow.style.left = this.arrowAtRight_ ? 'auto' : edgeOffset;
      bubbleArrow.style.right = this.arrowAtRight_ ? edgeOffset : 'auto';
    },
  };

  /**
   * A bubble that remains open until the user explicitly dismisses it or clicks
   * outside the bubble after it has been shown for at least the specified
   * amount of time (making it less likely that the user will unintentionally
   * dismiss the bubble). The bubble repositions itself on layout changes.
   * @constructor
   * @extends {cr.ui.BubbleBase}
   */
  const Bubble = cr.ui.define('div');

  Bubble.prototype = {
    // Set up the prototype chain.
    __proto__: BubbleBase.prototype,

    /**
     * Initialization function for the cr.ui framework.
     */
    decorate() {
      BubbleBase.prototype.decorate.call(this);

      const close = document.createElement('div');
      close.className = 'bubble-close';
      this.insertBefore(close, this.querySelector('.bubble-content'));

      this.handleCloseEvent = this.hide;
      this.deactivateToDismissDelay_ = 0;
      this.bubbleAlignment = cr.ui.BubbleAlignment.ARROW_TO_MID_ANCHOR;
    },

    /**
     * Handler for close events triggered when the close button is clicked. By
     * default, set to this.hide. Only available when the bubble is not being
     * shown.
     * @param {function(): *} handler The new handler, a function with no
     *     parameters.
     */
    set handleCloseEvent(handler) {
      if (!this.hidden) {
        return;
      }

      this.handleCloseEvent_ = handler;
    },

    /**
     * Set the delay before the user is allowed to click outside the bubble to
     * dismiss it. Using a delay makes it less likely that the user will
     * unintentionally dismiss the bubble.
     * @param {number} delay The delay in milliseconds.
     */
    set deactivateToDismissDelay(delay) {
      this.deactivateToDismissDelay_ = delay;
    },

    /**
     * Hide or show the close button.
     * @param {boolean} isVisible True if the close button should be visible.
     */
    set closeButtonVisible(isVisible) {
      this.querySelector('.bubble-close').hidden = !isVisible;
    },

    /**
     * Show the bubble.
     */
    show() {
      if (!this.hidden) {
        return;
      }

      BubbleBase.prototype.show.call(this);

      this.showTime_ = Date.now();
      this.eventTracker_.add(window, 'resize', this.reposition.bind(this));
    },

    /**
     * Handle keyboard and mouse events, dismissing the bubble if necessary.
     * @param {Event} event The event.
     * @suppress {checkTypes}
     * TODO(vitalyp): remove suppression when the extern
     * Node.prototype.contains() will be fixed.
     */
    handleEvent(event) {
      BubbleBase.prototype.handleEvent.call(this, event);

      if (event.type === 'mousedown') {
        // Dismiss the bubble when the user clicks on the close button.
        if (event.target === this.querySelector('.bubble-close')) {
          this.handleCloseEvent_();
          // Dismiss the bubble when the user clicks outside it after the
          // specified delay has passed.
        } else if (
            !this.contains(event.target) &&
            Date.now() - this.showTime_ >= this.deactivateToDismissDelay_) {
          this.hide();
        }
      }
    },
  };

  /**
   * A bubble that closes automatically when the user clicks or moves the focus
   * outside the bubble and its target element, scrolls the underlying document
   * or resizes the window.
   * @constructor
   * @extends {cr.ui.BubbleBase}
   */
  const AutoCloseBubble = cr.ui.define('div');

  AutoCloseBubble.prototype = {
    // Set up the prototype chain.
    __proto__: BubbleBase.prototype,

    /**
     * Initialization function for the cr.ui framework.
     */
    decorate() {
      BubbleBase.prototype.decorate.call(this);
      this.classList.add('auto-close-bubble');
    },

    /**
     * Set the DOM sibling node, i.e. the node as whose sibling the bubble
     * should join the DOM to ensure that focusable elements inside the bubble
     * follow the target element in the document's tab order. Only available
     * when the bubble is not being shown.
     * @param {HTMLElement} node The new DOM sibling node.
     */
    set domSibling(node) {
      if (!this.hidden) {
        return;
      }

      this.domSibling_ = node;
    },

    /**
     * Show the bubble.
     */
    show() {
      if (!this.hidden) {
        return;
      }

      BubbleBase.prototype.show.call(this);
      this.domSibling_.showingBubble = true;

      const doc = this.ownerDocument;
      this.eventTracker_.add(doc, 'click', this, true);
      this.eventTracker_.add(doc, 'mousewheel', this, true);
      this.eventTracker_.add(doc, 'scroll', this, true);
      this.eventTracker_.add(doc, 'elementFocused', this, true);
      this.eventTracker_.add(window, 'resize', this);
    },

    /**
     * Hide the bubble.
     */
    hide() {
      BubbleBase.prototype.hide.call(this);
      this.domSibling_.showingBubble = false;
    },

    /**
     * Handle events, closing the bubble when the user clicks or moves the focus
     * outside the bubble and its target element, scrolls the underlying
     * document or resizes the window.
     * @param {Event} event The event.
     * @suppress {checkTypes}
     * TODO(vitalyp): remove suppression when the extern
     * Node.prototype.contains() will be fixed.
     */
    handleEvent(event) {
      BubbleBase.prototype.handleEvent.call(this, event);

      let target;
      switch (event.type) {
        // Close the bubble when the user clicks outside it, except if it is a
        // left-click on the bubble's target element (allowing the target to
        // handle the event and close the bubble itself).
        case 'mousedown':
        case 'click':
          target = assertInstanceof(event.target, Node);
          if (event.button === 0 && this.anchorNode_.contains(target)) {
            break;
          }
        // Close the bubble when the underlying document is scrolled.
        case 'mousewheel':
        case 'scroll':
          target = assertInstanceof(event.target, Node);
          if (this.contains(target)) {
            break;
          }
        // Close the bubble when the window is resized.
        case 'resize':
          this.hide();
          break;
        // Close the bubble when the focus moves to an element that is not the
        // bubble target and is not inside the bubble.
        case 'elementFocused':
          target = assertInstanceof(event.target, Node);
          if (!this.anchorNode_.contains(target) && !this.contains(target)) {
            this.hide();
          }
          break;
      }
    },

    /**
     * Attach the bubble to the document's DOM, making it a sibling of the
     * |domSibling_| so that focusable elements inside the bubble follow the
     * target element in the document's tab order.
     * @private
     */
    attachToDOM_() {
      const parent = this.domSibling_.parentNode;
      parent.insertBefore(this, this.domSibling_.nextSibling);
    },
  };

  return {
    ArrowLocation: ArrowLocation,
    AutoCloseBubble: AutoCloseBubble,
    BubbleAlignment: BubbleAlignment,
    BubbleBase: BubbleBase,
    Bubble: Bubble,
  };
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// require: event_target.js

// clang-format off
// #import {assertInstanceof} from '../../assert.m.js';
// #import {NativeEventTarget as EventTarget} from '../event_target.m.js'
// #import {EventTracker} from '../../event_tracker.m.js'
// #import {isWindows, isLinux, isMac, isLacros, dispatchPropertyChange} from '../../cr.m.js';
// #import {decorate} from '../ui.m.js';
// #import {Menu} from './menu.m.js';
// #import {MenuItem} from './menu_item.m.js';
// #import {HideType} from './menu_button.m.js';
// #import {positionPopupAtPoint} from './position_util.m.js';
// clang-format on

cr.define('cr.ui', function() {
  /* #ignore */ /** @const */ const Menu = cr.ui.Menu;

  /**
   * Handles context menus.
   * @implements {EventListener}
   */
  class ContextMenuHandler extends cr.EventTarget {
    constructor() {
      super();
      /** @private {!EventTracker} */
      this.showingEvents_ = new EventTracker();

      /**
       * The menu that we are currently showing.
       * @private {?cr.ui.Menu}
       */
      this.menu_ = null;

      /** @private {?number} */
      this.hideTimestamp_ = null;

      /** @private {boolean} */
      this.keyIsDown_ = false;
    }

    get menu() {
      return this.menu_;
    }

    /**
     * Shows a menu as a context menu.
     * @param {!Event} e The event triggering the show (usually a contextmenu
     *     event).
     * @param {!cr.ui.Menu} menu The menu to show.
     */
    showMenu(e, menu) {
      menu.updateCommands(assertInstanceof(e.currentTarget, Node));
      if (!menu.hasVisibleItems()) {
        return;
      }

      this.menu_ = menu;
      menu.classList.remove('hide-delayed');
      menu.show({x: e.screenX, y: e.screenY});
      menu.contextElement = e.currentTarget;

      // When the menu is shown we steal a lot of events.
      const doc = menu.ownerDocument;
      const win = /** @type {!Window} */ (doc.defaultView);
      this.showingEvents_.add(doc, 'keydown', this, true);
      this.showingEvents_.add(doc, 'mousedown', this, true);
      this.showingEvents_.add(doc, 'touchstart', this, true);
      this.showingEvents_.add(doc, 'focus', this);
      this.showingEvents_.add(win, 'popstate', this);
      this.showingEvents_.add(win, 'resize', this);
      this.showingEvents_.add(win, 'blur', this);
      this.showingEvents_.add(menu, 'contextmenu', this);
      this.showingEvents_.add(menu, 'activate', this);
      this.positionMenu_(e, menu);

      const ev = new Event('show');
      ev.element = menu.contextElement;
      ev.menu = menu;
      this.dispatchEvent(ev);
    }

    /**
     * Hide the currently shown menu.
     * @param {cr.ui.HideType=} opt_hideType Type of hide.
     *     default: cr.ui.HideType.INSTANT.
     */
    hideMenu(opt_hideType) {
      const menu = this.menu;
      if (!menu) {
        return;
      }

      if (opt_hideType === cr.ui.HideType.DELAYED) {
        menu.classList.add('hide-delayed');
      } else {
        menu.classList.remove('hide-delayed');
      }
      menu.hide();
      const originalContextElement = menu.contextElement;
      menu.contextElement = null;
      this.showingEvents_.removeAll();
      menu.selectedIndex = -1;
      this.menu_ = null;

      // On windows we might hide the menu in a right mouse button up and if
      // that is the case we wait some short period before we allow the menu
      // to be shown again.
      this.hideTimestamp_ = cr.isWindows ? Date.now() : 0;

      const ev = new Event('hide');
      ev.element = originalContextElement;
      ev.menu = menu;
      this.dispatchEvent(ev);
    }

    /**
     * Positions the menu
     * @param {!Event} e The event object triggering the showing.
     * @param {!cr.ui.Menu} menu The menu to position.
     * @private
     */
    positionMenu_(e, menu) {
      // TODO(arv): Handle scrolled documents when needed.

      const element = e.currentTarget;
      let x, y;
      // When the user presses the context menu key (on the keyboard) we need
      // to detect this.
      if (this.keyIsDown_) {
        const rect = element.getRectForContextMenu ?
            element.getRectForContextMenu() :
            element.getBoundingClientRect();
        const offset = Math.min(rect.width, rect.height) / 2;
        x = rect.left + offset;
        y = rect.top + offset;
      } else {
        x = e.clientX;
        y = e.clientY;
      }

      cr.ui.positionPopupAtPoint(x, y, menu);
    }

    /**
     * Handles event callbacks.
     * @param {!Event} e The event object.
     */
    handleEvent(e) {
      // Keep track of keydown state so that we can use that to determine the
      // reason for the contextmenu event.
      switch (e.type) {
        case 'keydown':
          this.keyIsDown_ = !e.ctrlKey && !e.altKey &&
              // context menu key or Shift-F10
              (e.keyCode === 93 && !e.shiftKey ||
               e.key === 'F10' && e.shiftKey);
          break;

        case 'keyup':
          this.keyIsDown_ = false;
          break;
      }

      // Context menu is handled even when we have no menu.
      if (e.type !== 'contextmenu' && !this.menu) {
        return;
      }

      switch (e.type) {
        case 'mousedown':
          if (!this.menu.contains(e.target)) {
            this.hideMenu();
            if (e.button === 0 /* Left button */ &&
                (cr.isLinux || cr.isMac || cr.isLacros)) {
              // Emulate Mac and Linux, which swallow native 'mousedown' events
              // that close menus.
              e.preventDefault();
              e.stopPropagation();
            }
          } else {
            e.preventDefault();
          }
          break;

        case 'touchstart':
          if (!this.menu.contains(e.target)) {
            this.hideMenu();
          }
          break;

        case 'keydown':
          if (e.key === 'Escape') {
            this.hideMenu();
            e.stopPropagation();
            e.preventDefault();

            // If the menu is visible we let it handle all the keyboard events.
          } else if (this.menu) {
            this.menu.handleKeyDown(e);
            e.preventDefault();
            e.stopPropagation();
          }
          break;

        case 'activate':
          const hideDelayed =
              e.target instanceof cr.ui.MenuItem && e.target.checkable;
          this.hideMenu(
              hideDelayed ? cr.ui.HideType.DELAYED : cr.ui.HideType.INSTANT);
          break;

        case 'focus':
          if (!this.menu.contains(e.target)) {
            this.hideMenu();
          }
          break;

        case 'blur':
          this.hideMenu();
          break;

        case 'popstate':
        case 'resize':
          this.hideMenu();
          break;

        case 'contextmenu':
          if ((!this.menu || !this.menu.contains(e.target)) &&
              (!this.hideTimestamp_ || Date.now() - this.hideTimestamp_ > 50)) {
            this.showMenu(e, e.currentTarget.contextMenu);
          }
          e.preventDefault();
          // Don't allow elements further up in the DOM to show their menus.
          e.stopPropagation();
          break;
      }
    }

    /**
     * Adds a contextMenu property to an element or element class.
     * @param {!Element|!Function} elementOrClass The element or class to add
     *     the contextMenu property to.
     */
    addContextMenuProperty(elementOrClass) {
      const target = typeof elementOrClass === 'function' ?
          elementOrClass.prototype :
          elementOrClass;

      // eslint-disable-next-line no-restricted-properties
      target.__defineGetter__('contextMenu', function() {
        return this.contextMenu_;
      });
      // eslint-disable-next-line no-restricted-properties
      target.__defineSetter__('contextMenu', function(menu) {
        const oldContextMenu = this.contextMenu;

        if (typeof menu === 'string' && menu[0] === '#') {
          menu = this.ownerDocument.getElementById(menu.slice(1));
          cr.ui.decorate(menu, Menu);
        }

        if (menu === oldContextMenu) {
          return;
        }

        if (oldContextMenu && !menu) {
          this.removeEventListener('contextmenu', contextMenuHandler);
          this.removeEventListener('keydown', contextMenuHandler);
          this.removeEventListener('keyup', contextMenuHandler);
        }
        if (menu && !oldContextMenu) {
          this.addEventListener('contextmenu', contextMenuHandler);
          this.addEventListener('keydown', contextMenuHandler);
          this.addEventListener('keyup', contextMenuHandler);
        }

        this.contextMenu_ = menu;

        if (menu && menu.id) {
          this.setAttribute('contextmenu', '#' + menu.id);
        }

        cr.dispatchPropertyChange(this, 'contextMenu', menu, oldContextMenu);
      });

      if (!target.getRectForContextMenu) {
        /**
         * @return {!ClientRect} The rect to use for positioning the context
         *     menu when the context menu is not opened using a mouse position.
         */
        target.getRectForContextMenu = function() {
          return this.getBoundingClientRect();
        };
      }
    }

    /**
     * Sets the given contextMenu to the given element. A contextMenu property
     * would be added if necessary.
     * @param {!Element} element The element or class to set the contextMenu to.
     * @param {!cr.ui.Menu} contextMenu The contextMenu property to be set.
     */
    setContextMenu(element, contextMenu) {
      if (!element.contextMenu) {
        this.addContextMenuProperty(element);
      }
      element.contextMenu = contextMenu;
    }
  }

  /**
   * The singleton context menu handler.
   * @type {!ContextMenuHandler}
   */
  /* #export */ const contextMenuHandler = new ContextMenuHandler;

  // Export
  // #cr_define_end
  return {
    contextMenuHandler: contextMenuHandler,
  };
});
</script>
<script>// Copyright (c) 2011 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

/**
 * @fileoverview DragWrapper
 * A class for simplifying HTML5 drag and drop. Classes should use this to
 * handle the nitty gritty of nested drag enters and leaves.
 */
cr.define('cr.ui', function() {
  /** @interface */
  /* #export */ class DragWrapperDelegate {
    // TODO(devlin): The only method this "delegate" actually needs is
    // shouldAcceptDrag(); the rest can be events emitted by the DragWrapper.
    /**
     * @param {MouseEvent} e The event for the drag.
     * @return {boolean} Whether the drag should be accepted. If false,
     *     subsequent methods (doDrag*) will not be called.
     */
    shouldAcceptDrag(e) {}

    /** @param {MouseEvent} e */
    doDragEnter(e) {}

    /** @param {MouseEvent} e */
    doDragLeave(e) {}

    /** @param {MouseEvent} e */
    doDragOver(e) {}

    /** @param {MouseEvent} e */
    doDrop(e) {}
  }

  /**
   * Creates a DragWrapper which listens for drag target events on |target| and
   * delegates event handling to |delegate|.
   */
  /* #export */ class DragWrapper {
    /**
     * @param {!Element} target
     * @param {!cr.ui.DragWrapperDelegate} delegate
     */
    constructor(target, delegate) {
      /**
       * The number of un-paired dragenter events that have fired on |this|.
       * This is incremented by |onDragEnter_| and decremented by
       * |onDragLeave_|. This is necessary because dragging over child widgets
       * will fire additional enter and leave events on |this|. A non-zero value
       * does not necessarily indicate that |isCurrentDragTarget()| is true.
       * @private {number}
       */
      this.dragEnters_ = 0;

      /** @private {!Element} */
      this.target_ = target;

      /** @private {!cr.ui.DragWrapperDelegate} */
      this.delegate_ = delegate;

      target.addEventListener(
          'dragenter', e => this.onDragEnter_(/** @type {!MouseEvent} */ (e)));
      target.addEventListener(
          'dragover', e => this.onDragOver_(/** @type {!MouseEvent} */ (e)));
      target.addEventListener(
          'drop', e => this.onDrop_(/** @type {!MouseEvent} */ (e)));
      target.addEventListener(
          'dragleave', e => this.onDragLeave_(/** @type {!MouseEvent} */ (e)));
    }

    /**
     * Whether the tile page is currently being dragged over with data it can
     * accept.
     * @return {boolean}
     */
    get isCurrentDragTarget() {
      return this.target_.classList.contains('drag-target');
    }

    /**
     * Delegate for dragenter events fired on |target_|.
     * @param {!MouseEvent} e A MouseEvent for the drag.
     * @private
     */
    onDragEnter_(e) {
      if (++this.dragEnters_ === 1) {
        if (this.delegate_.shouldAcceptDrag(e)) {
          this.target_.classList.add('drag-target');
          this.delegate_.doDragEnter(e);
        }
      } else {
        // Sometimes we'll get an enter event over a child element without an
        // over event following it. In this case we have to still call the
        // drag over delegate so that we make the necessary updates (one visible
        // symptom of not doing this is that the cursor's drag state will
        // flicker during drags).
        this.onDragOver_(e);
      }
    }

    /**
     * Thunk for dragover events fired on |target_|.
     * @param {!MouseEvent} e A MouseEvent for the drag.
     * @private
     */
    onDragOver_(e) {
      if (!this.target_.classList.contains('drag-target')) {
        return;
      }
      this.delegate_.doDragOver(e);
    }

    /**
     * Thunk for drop events fired on |target_|.
     * @param {!MouseEvent} e A MouseEvent for the drag.
     * @private
     */
    onDrop_(e) {
      this.dragEnters_ = 0;
      if (!this.target_.classList.contains('drag-target')) {
        return;
      }
      this.target_.classList.remove('drag-target');
      this.delegate_.doDrop(e);
    }

    /**
     * Thunk for dragleave events fired on |target_|.
     * @param {!MouseEvent} e A MouseEvent for the drag.
     * @private
     */
    onDragLeave_(e) {
      if (--this.dragEnters_ > 0) {
        return;
      }

      this.target_.classList.remove('drag-target');
      this.delegate_.doDragLeave(e);
    }
  }

  // #cr_define_end
  return {
    DragWrapper: DragWrapper,
    DragWrapperDelegate: DragWrapperDelegate,
  };
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

cr.define('cr.ui', function() {
  /**
   * Constructor for FocusManager singleton. Checks focus of elements to ensure
   * that elements in "background" pages (i.e., those in a dialog that is not
   * the topmost overlay) do not receive focus.
   * @constructor
   */
  function FocusManager() {}

  FocusManager.prototype = {
    /**
     * Whether focus is being transferred backward or forward through the DOM.
     * @type {boolean}
     * @private
     */
    focusDirBackwards_: false,

    /**
     * Determines whether the |child| is a descendant of |parent| in the page's
     * DOM.
     * @param {Node} parent The parent element to test.
     * @param {Node} child The child element to test.
     * @return {boolean} True if |child| is a descendant of |parent|.
     * @private
     */
    isDescendantOf_(parent, child) {
      return !!parent && !(parent === child) && parent.contains(child);
    },

    /**
     * Returns the parent element containing all elements which should be
     * allowed to receive focus.
     * @return {Element} The element containing focusable elements.
     */
    getFocusParent() {
      return document.body;
    },

    /**
     * Returns the elements on the page capable of receiving focus.
     * @return {Array<Element>} The focusable elements.
     */
    getFocusableElements_() {
      const focusableDiv = this.getFocusParent();

      // Create a TreeWalker object to traverse the DOM from |focusableDiv|.
      const treeWalker = document.createTreeWalker(
          focusableDiv, NodeFilter.SHOW_ELEMENT,
          /** @type {NodeFilter} */
          ({
            acceptNode(node) {
              const style = window.getComputedStyle(node);
              // Reject all hidden nodes. FILTER_REJECT also rejects these
              // nodes' children, so non-hidden elements that are descendants of
              // hidden <div>s will correctly be rejected.
              if (node.hidden || style.display === 'none' ||
                  style.visibility === 'hidden') {
                return NodeFilter.FILTER_REJECT;
              }

              // Skip nodes that cannot receive focus. FILTER_SKIP does not
              // cause this node's children also to be skipped.
              if (node.disabled || node.tabIndex < 0) {
                return NodeFilter.FILTER_SKIP;
              }

              // Accept nodes that are non-hidden and focusable.
              return NodeFilter.FILTER_ACCEPT;
            }
          }),
          false);

      const focusable = [];
      while (treeWalker.nextNode()) {
        focusable.push(treeWalker.currentNode);
      }

      return focusable;
    },

    /**
     * Dispatches an 'elementFocused' event to notify an element that it has
     * received focus. When focus wraps around within the a page, only the
     * element that has focus after the wrapping receives an 'elementFocused'
     * event. This differs from the native 'focus' event which is received by
     * an element outside the page first, followed by a 'focus' on an element
     * within the page after the FocusManager has intervened.
     * @param {EventTarget} element The element that has received focus.
     * @private
     */
    dispatchFocusEvent_(element) {
      cr.dispatchSimpleEvent(element, 'elementFocused', true, false);
    },

    /**
     * Attempts to focus the appropriate element in the current dialog.
     * @private
     */
    setFocus_() {
      const element = this.selectFocusableElement_();
      if (element) {
        element.focus();
        this.dispatchFocusEvent_(element);
      }
    },

    /**
     * Selects first appropriate focusable element according to the
     * current focus direction and element type.  If it is a radio button,
     * checked one is selected from the group.
     * @private
     */
    selectFocusableElement_() {
      // If |this.focusDirBackwards_| is true, the user has pressed "Shift+Tab"
      // and has caused the focus to be transferred backward, outside of the
      // current dialog. In this case, loop around and try to focus the last
      // element of the dialog; otherwise, try to focus the first element of the
      // dialog.
      const focusableElements = this.getFocusableElements_();
      let element = this.focusDirBackwards_ ? focusableElements.pop() :
                                              focusableElements.shift();
      if (!element) {
        return null;
      }
      if (element.tagName !== 'INPUT' || element.type !== 'radio' ||
          element.name === '') {
        return element;
      }
      if (!element.checked) {
        for (let i = 0; i < focusableElements.length; i++) {
          const e = focusableElements[i];
          if (e && e.tagName === 'INPUT' && e.type === 'radio' &&
              e.name === element.name && e.checked) {
            element = e;
            break;
          }
        }
      }
      return element;
    },

    /**
     * Handler for focus events on the page.
     * @param {Event} event The focus event.
     * @private
     */
    onDocumentFocus_(event) {
      // If the element being focused is a descendant of the currently visible
      // page, focus is valid.
      const targetNode = /** @type {Node} */ (event.target);
      if (this.isDescendantOf_(this.getFocusParent(), targetNode)) {
        this.dispatchFocusEvent_(event.target);
        return;
      }

      // Focus event handlers for descendant elements might dispatch another
      // focus event.
      event.stopPropagation();

      // The target of the focus event is not in the topmost visible page and
      // should not be focused.
      event.target.blur();

      // Attempt to wrap around focus within the current page.
      this.setFocus_();
    },

    /**
     * Handler for keydown events on the page.
     * @param {Event} event The keydown event.
     * @private
     */
    onDocumentKeyDown_(event) {
      /** @const */ const tabKeyCode = 9;

      if (event.keyCode === tabKeyCode) {
        // If the "Shift" key is held, focus is being transferred backward in
        // the page.
        this.focusDirBackwards_ = event.shiftKey ? true : false;
      }
    },

    /**
     * Initializes the FocusManager by listening for events in the document.
     */
    initialize() {
      document.addEventListener(
          'focus', this.onDocumentFocus_.bind(this), true);
      document.addEventListener(
          'keydown', this.onDocumentKeyDown_.bind(this), true);
    },
  };

  return {
    FocusManager: FocusManager,
  };
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// clang-format off
// #import {loadTimeData} from '../../load_time_data.m.js';
// #import {assert} from '../../assert.m.js';
// #import {Command} from './command.m.js';
// #import {define as crUiDefine, decorate, swallowDoubleClick} from '../ui.m.js';
// #import {getPropertyDescriptor, PropertyKind} from '../../cr.m.js';
// clang-format on

cr.define('cr.ui', function() {
  /* #ignore */ const Command = cr.ui.Command;

  /**
   * Creates a new menu item element.
   * @param {Object=} opt_propertyBag Optional properties.
   * @constructor
   * @extends {HTMLElement}
   * @implements {EventListener}
   */
  /* #export */ const MenuItem = cr.ui.define('cr-menu-item');

  /**
   * Creates a new menu separator element.
   * @return {!cr.ui.MenuItem} The new separator element.
   */
  MenuItem.createSeparator = function() {
    const el = /** @type {!cr.ui.MenuItem} */ (document.createElement('hr'));
    if (MenuItem.decorate) {
      MenuItem.decorate(el);
    }
    return el;
  };

  MenuItem.prototype = {
    __proto__: HTMLElement.prototype,

    /**
     * Initializes the menu item.
     */
    decorate() {
      let commandId;
      if ((commandId = this.getAttribute('command'))) {
        this.command = commandId;
      }

      this.addEventListener('mouseup', this.handleMouseUp_);

      // Adding the 'custom-appearance' class prevents widgets.css from changing
      // the appearance of this element.
      this.classList.add('custom-appearance');

      // Enable Text to Speech on the menu. Additionaly, ID has to be set, since
      // it is used in element's aria-activedescendant attribute.
      if (!this.isSeparator()) {
        this.setAttribute('role', 'menuitem');
        this.setAttribute('tabindex', this.getAttribute('tabindex') || -1);
      }

      let iconUrl;
      if ((iconUrl = this.getAttribute('icon'))) {
        this.iconUrl = iconUrl;
      }
    },

    /**
     * The command associated with this menu item. If this is set to a string
     * of the form "#element-id" then the element is looked up in the document
     * of the command.
     * @type {cr.ui.Command}
     */
    command_: null,
    get command() {
      return this.command_;
    },
    set command(command) {
      if (this.command_) {
        this.command_.removeEventListener('labelChange', this);
        this.command_.removeEventListener('disabledChange', this);
        this.command_.removeEventListener('hiddenChange', this);
        this.command_.removeEventListener('checkedChange', this);
      }

      if (typeof command === 'string' && command[0] === '#') {
        command = assert(this.ownerDocument.body.querySelector(command));
        cr.ui.decorate(command, Command);
      }

      this.command_ = command;
      if (command) {
        if (command.id) {
          this.setAttribute('command', '#' + command.id);
        }

        if (typeof command.label === 'string') {
          this.label = command.label;
        }
        this.disabled = command.disabled;
        this.hidden = command.hidden;
        this.checked = command.checked;

        this.command_.addEventListener('labelChange', this);
        this.command_.addEventListener('disabledChange', this);
        this.command_.addEventListener('hiddenChange', this);
        this.command_.addEventListener('checkedChange', this);
      }

      this.updateShortcut_();
    },

    /**
     * The text label.
     * @type {string}
     */
    get label() {
      return this.textContent;
    },
    set label(label) {
      this.textContent = label;
    },

    /**
     * Menu icon.
     * @type {string}
     */
    get iconUrl() {
      return this.style.backgroundImage;
    },
    set iconUrl(url) {
      this.style.backgroundImage = 'url(' + url + ')';
    },

    /**
     * @return {boolean} Whether the menu item is a separator.
     */
    isSeparator() {
      return this.tagName === 'HR';
    },

    /**
     * Updates shortcut text according to associated command. If command has
     * multiple shortcuts, only first one is displayed.
     */
    updateShortcut_() {
      this.removeAttribute('shortcutText');

      if (!this.command_ || !this.command_.shortcut ||
          this.command_.hideShortcutText) {
        return;
      }

      const shortcuts = this.command_.shortcut.split(/\s+/);

      if (shortcuts.length === 0) {
        return;
      }

      const shortcut = shortcuts[0];
      const mods = {};
      let ident = '';
      shortcut.split('|').forEach(function(part) {
        const partUc = part.toUpperCase();
        switch (partUc) {
          case 'CTRL':
          case 'ALT':
          case 'SHIFT':
          case 'META':
            mods[partUc] = true;
            break;
          default:
            console.assert(!ident, 'Shortcut has two non-modifier keys');
            ident = part;
        }
      });

      let shortcutText = '';

      ['CTRL', 'ALT', 'SHIFT', 'META'].forEach(function(mod) {
        if (mods[mod]) {
          shortcutText += loadTimeData.getString('SHORTCUT_' + mod) + '+';
        }
      });

      if (ident === ' ') {
        ident = 'Space';
      }

      if (ident.length !== 1) {
        shortcutText +=
            loadTimeData.getString('SHORTCUT_' + ident.toUpperCase());
      } else {
        shortcutText += ident.toUpperCase();
      }

      this.setAttribute('shortcutText', shortcutText);
    },

    /**
     * Handles mouseup events. This dispatches an activate event; if there is an
     * associated command, that command is executed.
     * @param {!Event} e The mouseup event object.
     * @private
     */
    handleMouseUp_(e) {
      e = /** @type {!MouseEvent} */ (e);
      // Only dispatch an activate event for left or middle click.
      if (e.button > 1) {
        return;
      }

      if (!this.disabled && !this.isSeparator() && this.selected) {
        // Store |contextElement| since it'll be removed by {Menu} on handling
        // 'activate' event.
        const contextElement =
            /** @type {{contextElement: Element}} */ (this.parentNode)
                .contextElement;
        const activationEvent = document.createEvent('Event');
        activationEvent.initEvent('activate', true, true);
        activationEvent.originalEvent = e;
        // Dispatch command event followed by executing the command object.
        if (this.dispatchEvent(activationEvent)) {
          const command = this.command;
          if (command) {
            command.execute(contextElement);
            cr.ui.swallowDoubleClick(e);
          }
        }
      }
    },

    /**
     * Updates command according to the node on which this menu was invoked.
     * @param {Node=} opt_node Node on which menu was opened.
     */
    updateCommand(opt_node) {
      if (this.command_) {
        this.command_.canExecuteChange(opt_node);
      }
    },

    /**
     * Handles changes to the associated command.
     * @param {Event} e The event object.
     */
    handleEvent(e) {
      switch (e.type) {
        case 'disabledChange':
          this.disabled = this.command.disabled;
          break;
        case 'hiddenChange':
          this.hidden = this.command.hidden;
          break;
        case 'labelChange':
          this.label = this.command.label;
          break;
        case 'checkedChange':
          this.checked = this.command.checked;
          break;
      }
    }
  };
  /**
   * Whether the menu item is disabled or not.
   * @type {boolean}
   */
  MenuItem.prototype.disabled;
  Object.defineProperty(
      MenuItem.prototype, 'disabled',
      cr.getPropertyDescriptor('disabled', cr.PropertyKind.BOOL_ATTR));

  /**
   * Whether the menu item is hidden or not.
   */
  Object.defineProperty(
      MenuItem.prototype, 'hidden',
      cr.getPropertyDescriptor('hidden', cr.PropertyKind.BOOL_ATTR));

  /**
   * Whether the menu item is selected or not.
   * @type {boolean}
   */
  MenuItem.prototype.selected;
  Object.defineProperty(
      MenuItem.prototype, 'selected',
      cr.getPropertyDescriptor('selected', cr.PropertyKind.BOOL_ATTR));

  /**
   * Whether the menu item is checked or not.
   * @type {boolean}
   */
  MenuItem.prototype.checked;
  Object.defineProperty(
      MenuItem.prototype, 'checked',
      cr.getPropertyDescriptor('checked', cr.PropertyKind.BOOL_ATTR));

  /**
   * Whether the menu item is checkable or not.
   * @type {boolean}
   */
  MenuItem.prototype.checkable;
  Object.defineProperty(
      MenuItem.prototype, 'checkable',
      cr.getPropertyDescriptor('checkable', cr.PropertyKind.BOOL_ATTR));

  // Export
  // #cr_define_end
  return {MenuItem: MenuItem};
});
</script>
<script>// Copyright (c) 2012 The Chromium Authors. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

// #import {assert, assertInstanceof} from '../../assert.m.js';
// #import {define as crUiDefine, decorate} from '../ui.m.js';
// #import {getPropertyDescriptor, PropertyKind} from '../../cr.m.js';
// #import {MenuItem} from './menu_item.m.js';

cr.define('cr.ui', function() {
  /* #ignore */ /** @const */ const MenuItem = cr.ui.MenuItem;

  /**
   * Creates a new menu element. Menu dispatches all commands on the element it
   * was shown for.
   *
   * @param {Object=} opt_propertyBag Optional properties.
   * @constructor
   * @extends {HTMLElement}
   */
  /* #export */ const Menu = cr.ui.define('cr-menu');

  Menu.prototype = {
    __proto__: HTMLElement.prototype,

    selectedIndex_: -1,

    /**
     * Element for which menu is being shown.
     */
    contextElement: null,

    /**
     * Initializes the menu element.
     */
    decorate() {
      this.addEventListener('mouseover', this.handleMouseOver_);
      this.addEventListener('mouseout', this.handleMouseOut_);
      this.addEventListener('mouseup', this.handleMouseUp_, true);

      this.classList.add('decorated');
      this.setAttribute('role', 'menu');
      this.hidden = true;  // Hide the menu by default.

      // Decorate the children as menu items.
      const menuItems = this.menuItems;
      for (let i = 0, menuItem; menuItem = menuItems[i]; i++) {
        cr.ui.decorate(menuItem, MenuItem);
      }
    },

    /**
     * Adds menu item at the end of the list.
     * @param {Object} item Menu item properties.
     * @return {!cr.ui.MenuItem} The created menu item.
     */
    addMenuItem(item) {
      const menuItem = this.ownerDocument.createElement('cr-menu-item');
      this.appendChild(menuItem);

      cr.ui.decorate(menuItem, MenuItem);

      if (item.label) {
        menuItem.label = item.label;
      }

      if (item.iconUrl) {
        menuItem.iconUrl = item.iconUrl;
      }

      return menuItem;
    },

    /**
     * Adds separator at the end of the list.
     */
    addSeparator() {
      const separator = this.ownerDocument.createElement('hr');
      cr.ui.decorate(separator, MenuItem);
      this.appendChild(separator);
    },

    /**
     * Clears menu.
     */
    clear() {
      this.selectedItem = null;
      this.textContent = '';
    },

    /**
     * Walks up the ancestors of |node| until a menu item belonging to this menu
     * is found.
     * @param {Node} node The node to start searching from.
     * @return {cr.ui.MenuItem} The found menu item or null.
     * @private
     */
    findMenuItem_(node) {
      while (node && node.parentNode !== this && !(node instanceof MenuItem)) {
        node = node.parentNode;
      }
      return node ? assertInstanceof(node, MenuItem) : null;
    },

    /**
     * Handles mouseover events and selects the hovered item.
     * @param {Event} e The mouseover event.
     * @private
     */
    handleMouseOver_(e) {
      const overItem = this.findMenuItem_(/** @type {Element} */ (e.target));
      this.selectedItem = overItem;
    },

    /**
     * Handles mouseout events and deselects any selected item.
     * @param {Event} e The mouseout event.
     * @private
     */
    handleMouseOut_(e) {
      this.selectedItem = null;
    },

    /**
     * If there's a mouseup that happens quickly in about the same position,
     * stop it from propagating to items. This is to prevent accidentally
     * selecting a menu item that's created under the mouse cursor.
     * @param {Event} e A mouseup event on the menu (in capturing phase).
     * @private
     */
    handleMouseUp_(e) {
      assert(this.contains(/** @type {Element} */ (e.target)));

      if (!this.trustEvent_(e) || Date.now() - this.shown_.time > 200) {
        return;
      }

      const pos = this.shown_.mouseDownPos;
      if (!pos ||
          Math.abs(pos.x - e.screenX) + Math.abs(pos.y - e.screenY) > 4) {
        return;
      }

      e.preventDefault();
      e.stopPropagation();
    },

    /**
     * @param {!Event} e
     * @return {boolean} Whether |e| can be trusted.
     * @private
     * @suppress {checkTypes}
     */
    trustEvent_(e) {
      return e.isTrusted || e.isTrustedForTesting;
    },

    get menuItems() {
      return this.querySelectorAll(this.menuItemSelector || '*');
    },

    /**
     * The selected menu item or null if none.
     * @type {cr.ui.MenuItem}
     */
    get selectedItem() {
      return this.menuItems[this.selectedIndex];
    },
    set selectedItem(item) {
      const index = Array.prototype.indexOf.call(this.menuItems, item);
      this.selectedIndex = index;
    },

    /**
     * Focuses the selected item. If selectedIndex is invalid, set it to 0
     * first.
     */
    focusSelectedItem() {
      const items = this.menuItems;
      if (this.selectedIndex < 0 || this.selectedIndex > items.length) {
        // Find first visible item to focus by default.
        for (let idx = 0; idx < items.length; idx++) {
          const item = items[idx];
          if (item.hasAttribute('hidden') || item.isSeparator()) {
            continue;
          }
          // If the item is disabled we accept it, but try to find the next
          // enabled item, but keeping the first disabled item.
          if (!item.disabled) {
            this.selectedIndex = idx;
            break;
          } else if (this.selectedIndex === -1) {
            this.selectedIndex = idx;
          }
        }
      }

      if (this.selectedItem) {
        this.selectedItem.focus();
        this.setAttribute('aria-activedescendant', this.selectedItem.id);
      }
    },

    /**
     * Menu length
     */
    get length() {
      return this.menuItems.length;
    },

    /**
     * Returns whether the given menu item is visible.
     * @param {!cr.ui.MenuItem} menuItem
     * @return {boolean}
     * @private
     */
    isItemVisible_(menuItem) {
      if (menuItem.hidden) {
        return false;
      }
      if (menuItem.offsetParent) {
        return true;
      }
      // A "position: fixed" element won't have an offsetParent, so we have to
      // do the full style computation.
      return window.getComputedStyle(menuItem).display !== 'none';
    },

    /**
     * Returns whether the menu has any visible items.
     * @return {boolean} True if the menu has visible item. Otherwise, false.
     */
    hasVisibleItems() {
      // Inspect items in reverse order to determine if the separator above each
      // set of items is required.
      for (const menuItem of this.menuItems) {
        if (this.isItemVisible_(menuItem)) {
          return true;
        }
      }
      return false;
    },

    /**
     * This is the function that handles keyboard navigation. This is usually
     * called by the element responsible for managing the menu.
     * @param {Event} e The keydown event object.
     * @return {boolean} Whether the event was handled be the menu.
     */
    handleKeyDown(e) {
      let item = this.selectedItem;

      const self = this;
      const selectNextAvailable = function(m) {
        const menuItems = self.menuItems;
        const len = menuItems.length;
        if (!len) {
          // Edge case when there are no items.
          return;
        }
        let i = self.selectedIndex;
        if (i === -1 && m === -1) {
          // Edge case when needed to go the last item first.
          i = 0;
        }

        // "i" may be negative(-1), so modulus operation and cycle below
        // wouldn't work as assumed. This trick makes startPosition positive
        // without altering it's modulo.
        const startPosition = (i + len) % len;

        while (true) {
          i = (i + m + len) % len;

          // Check not to enter into infinite loop if all items are hidden or
          // disabled.
          if (i === startPosition) {
            break;
          }

          item = menuItems[i];
          if (item && !item.isSeparator() && !item.disabled &&
              this.isItemVisible_(item)) {
            break;
          }
        }
        if (item && !item.disabled) {
          self.selectedIndex = i;
        }
      }.bind(this);

      switch (e.key) {
        case 'ArrowDown':
          selectNextAvailable(1);
          this.focusSelectedItem();
          return true;
        case 'ArrowUp':
          selectNextAvailable(-1);
          this.focusSelectedItem();
          return true;
        case 'Enter':
        case ' ':
          if (item) {
            // Store |contextElement| since it'll be removed when handling the
            // 'activate' event.
            const contextElement = this.contextElement;
            const activationEvent = document.createEvent('Event');
            activationEvent.initEvent('activate', true, true);
            activationEvent.originalEvent = e;
            if (item.dispatchEvent(activationEvent)) {
              if (item.command) {
                item.command.execute(contextElement);
              }
            }
          }
          return true;
      }

      return false;
    },

    hide() {
      this.hidden = true;
      delete this.shown_;
    },

    /** @param {{x: number, y: number}=} opt_mouseDownPos */
    show(opt_mouseDownPos) {
      this.shown_ = {mouseDownPos: opt_mouseDownPos, time: Date.now()};
      this.hidden = false;
    },

    /**
     * Updates menu items command according to context.
     * @param {Node=} node Node for which to actuate commands state.
     */
    updateCommands(node) {
      const menuItems = this.menuItems;

      for (const menuItem of menuItems) {
        if (!menuItem.isSeparator()) {
          menuItem.updateCommand(node);
        }
      }

      let separatorRequired = false;
      let lastSeparator = null;
      // Hide any separators without a visible item between them and the next
      // separator or the end of the menu.
      for (const menuItem of menuItems) {
        if (menuItem.isSeparator()) {
          if (separatorRequired) {
            lastSeparator = menuItem;
          }
          menuItem.hidden = true;
          separatorRequired = false;
          continue;
        }
        if (this.isItemVisible_(menuItem)) {
          if (lastSeparator) {
            lastSeparator.hidden = false;
          }
          separatorRequired = true;
        }
      }
    }
  };

  /** @suppress {globalThis} This standalone function is used like method. */
  function selectedIndexChanged(selectedIndex, oldSelectedIndex) {
