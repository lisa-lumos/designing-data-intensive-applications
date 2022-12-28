# 4. Encoding and Evolution
Old and new versions of the code, and old and new data formats, may coexist in the system at the same time. For the system to continue running smoothly, we need to maintain compatibility in both directions:
- Backward compatibility: Newer code can read old data. Easy to achieve, because you know the old data format
- Forward compatibility: Older code can read new data. Harder to achieve, because you need to ignore future additions in data format

The translation from the in-memory representation to a byte sequence is called `encoding` (serialization/marshalling), and the reverse is called `decoding` (parsing/ deserialization/unmarshalling).

Language-specific formats for encoding: convenient, but lock you to a particular programming language; be a security issue; back forward/backward compatibility; bad performance. => it’s usually not recommended to use your language’s built-in encoding. 

JSON, XML, and Binary Variants: JSON, XML, and CSV are textual formats, and thus human-readable. Problems: Ambiguity: Without external schema, XML & CSV cannot distinguish a num and a digit-only string; JSON cannot specify a precision, and cannot distinguish integers and floats. JSON and XML do not support binary strings, so people use base64 encoding. 

Binary encoding: 







































