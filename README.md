# 🔐 Next.js Code Obfuscation using `webpack-obfuscator`

Obfuscating your JavaScript code adds an extra layer of protection by making the output bundle harder to understand and reverse-engineer. This guide explains how to use `webpack-obfuscator` in a Next.js project to secure your client-side code in production.

---

## 📦 Prerequisites

Before getting started, ensure the following are already set up:

- [Node.js](https://nodejs.org/en/) (v14 or newer)
- A Next.js project (TypeScript or JavaScript)
- TypeScript configuration (optional but recommended)

---

## 🚀 Installation & Configuration

To begin, install `webpack-obfuscator` as a **dev dependency** by running:

```bash
npm install --save-dev webpack-obfuscator
# or
yarn add --dev webpack-obfuscator
```

Then, in your Next.js project root, locate or create a `next.config.ts` (or `next.config.js` for JavaScript) and add the following configuration:

```ts
// next.config.ts

const nextConfig: NextConfig = {
  webpack: (config, { dev, isServer }) => {
    if (!dev && !isServer) {
      config.plugins?.push(
        new WebpackObfuscator(
          {
            compact: true,
            controlFlowFlattening: true,
            controlFlowFlatteningThreshold: 0.75,
            deadCodeInjection: true,
            deadCodeInjectionThreshold: 0.4,
            debugProtection: true,
            debugProtectionInterval: 2000,
            disableConsoleOutput: true,
            identifierNamesGenerator: "hexadecimal",
            log: false,
            numbersToExpressions: true,
            ...//You can add more according to your needs
          },
          [
            "node_modules/**",
            ".next/**",
            "public/**"
          ]
        )
      );
    }
    return config;
  },
};

```

> 💡 **Note**: If you're using JavaScript, remove type annotations and replace `import` with `require()` syntax accordingly.

---

## 🏗️ Usage

Once installed and configured, you can obfuscate your code by simply running a production build:

```bash
npm run build
# or
yarn build
```

This will apply obfuscation to your client-side JavaScript output, which can be found in the `.next/static/chunks` directory. You'll notice the code is minified, encoded, and no longer human-readable.

---

## ⚠️ Important Notes

- Obfuscation **only runs in production** (`next build`) and applies only to **client-side code**.
- It does **not** protect server-side logic or Next.js API routes.
- This may **increase your build time and bundle size**.
- Obfuscation is **not a security guarantee** — do not store secrets or sensitive data in frontend code.
- Compatible with both **JavaScript and TypeScript** projects.

---

## ✅ Summary

You’ve now successfully set up `webpack-obfuscator` to enhance the security of your frontend code in a Next.js app. While not foolproof, it adds meaningful complexity to deter casual reverse-engineering attempts.

Happy coding! 🔐
