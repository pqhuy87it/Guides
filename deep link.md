# 1. Verify Apple App Site Association

```bash
https://example.com/.well-known/assetlinks.json
```

# 2.Use Apple’s Validation Command

```bash
curl -v https://example.com/apple-app-site-association
```

# 3. Test in your app

## The cmd Command

```bash
xcrun simctl openurl booted myapp://screen?data=abc
```

## Opening Safari 

```bash
myapp://screen?data=abc
```
