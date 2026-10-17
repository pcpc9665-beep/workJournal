**Zod** is ==a TypeScript-first schema declaration and validation library==. It allows developers to define data structures (schemas) and validate untrusted data at runtime (such as API responses, form inputs, or environment variables) while automatically inferring static TypeScript types. [[1](https://zod.dev/), [2](https://www.totaltypescript.com/tutorials/zod), [3](https://www.telerik.com/blogs/zod-typescript-schema-validation-made-easy), [4](https://www.youtube.com/watch?v=U9PYyMhDc_k&t=1429)]

Key Features

- **Single Source of Truth:** You declare a Zod schema once, and it handles both runtime validation and TypeScript type extraction (using `z.infer<>`).
- **Zero External Dependencies:** The core library is lightweight (around 2kb gzipped) and runs in Node.js, Deno, and modern web browsers.
- **Immutability:** Schema modification methods (like `.optional()`, `.default()`, or `.array()`) return new schema instances rather than mutating existing ones.
- **Data Parsing & Transformation:** It doesn't just check if data is valid; it can parse, coerce, and transform input data (e.g., trimming strings, converting strings to numbers).
- **Ecosystem Integration:** Commonly paired with UI and full-stack tools like React Hook Form and [tRPC](https://zod.dev/). [[1](https://www.youtube.com/watch?v=YmFz19RVM4E), [2](https://www.contentful.com/blog/react-hook-form-validation-zod/), [3](https://www.telerik.com/blogs/zod-typescript-schema-validation-made-easy), [4](https://www.totaltypescript.com/tutorials/zod), [5](https://zod.dev/)]