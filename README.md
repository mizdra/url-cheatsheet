# url-cheatsheet

URL manipulation cheatsheet for JavaScript.

## DO NOT

Please DO NOT concat url and parameters as string.

```js
// THIS EXAMPLE IS UNSAFE
const query = "<user input>"
const url = `https://example.com?q=${query}`;
```

This example does not consider that `query` includes `&` or `?` that is required to escape.
Basically, you should not concat url as string. It is unsafe.

## Base URL + Path 

Use [`new URL(pathname, base)`](https://developer.mozilla.org/docs/Web/API/URL/URL).

- keywords: url-join, join path

```js
const base = "https://example.com";
const pathname = "/path/to/page";
const result = new URL(pathname, base);
console.log(result.toString()); // https://example.com/path/to/page
```

## Add parameters to URL

Use [URLSearchParams](https://developer.mozilla.org/docs/Web/API/URLSearchParams)

```js
const q = "query";
const page = 1;
const base = "https://example.com";
const url = new URL(base);
const params = new URLSearchParams({
    q,
    page
});
console.log(url + "?" + params); // https://example.com/?q=query&page=1
```

or 

```js
const q = "query";
const page = 1;
const base = "https://example.com";
const url = new URL(base);
url.search = new URLSearchParams({
    q,
    page
});
console.log(url.toString()); // https://example.com/?q=query&page=1
```

## Get parameter from URL

Use [`URL`](https://developer.mozilla.org/docs/Web/API/URL/URL) and [URLSearchParams](https://developer.mozilla.org/docs/Web/API/URLSearchParams)

```js
const inputURL = "https://example.com/?q=query&page=1";
const url = new URL(inputURL);
const q = url.searchParams.get("q");
console.log(q); // query
```

## Remove parameter from URL

Use [`URL`](https://developer.mozilla.org/docs/Web/API/URL/URL) and [URLSearchParams](https://developer.mozilla.org/docs/Web/API/URLSearchParams)

```js
const inputURL = "https://example.com/?q=query&page=1";
const url = new URL(inputURL);
url.searchParams.delete("q");
console.log(url.toString()); // "https://example.com/?page=1"
```

## Filter parameters

Allow only `a` and `d` parameters.

- keywords: pick, white list, allow list

```js
const base = "https://example.com/?a=1&b=2&c=3&d=4";
const url = new URL(base);
const allowedParameterNames = ["a", "d"];
url.search = new URLSearchParams(Array.from(url.searchParams).filter(([key, value]) => {
  return allowedParameterNames.includes(key);
}));
console.log(url.toString()); // "https://example.com/?a=1&d=4"
```

## Validation

Verify if `TypeError` is thrown when calling [`new URL(url)`](https://developer.mozilla.org/docs/Web/API/URL/URL).

```js
function isValidURL(str) {
  try {
    new URL(str);
    return true;
  } catch (e) {
    if (e instanceof TypeError) return false;
    throw e;
  }
}

console.log(isValidURL("https://example.com/")); // true
console.log(isValidURL("invalid string")); // false
```
