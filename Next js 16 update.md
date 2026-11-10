
Select **Yes** (Recommended). [[1](https://www.google.com/goto?url=CAESfQHrOzAVEZedUQmCP0HBlL0S2VFz3mpGsVETj_4r_CwQcRR9cW5W_KZ9rsoC5wN_YTVnEcvayGEPowOn_aASqOV-bcnKtszoNFRSCGy6Cn8PAeU-iYhN_yVAyPbd-DeQYuW6OY-9hAwFowE84WJcbifI9SoCmcH4eSMeov6d)]

**Cache Components** is the standardized architecture introduced in Next.js 16. It replaces old, confusing page-level configurations with granular, opt-in component-level caching. The Vercel team officially recommends it as the best choice for new projects. [[1](https://www.google.com/goto?url=CAESaQHrOzAVNHdyoMBpoc0b2Enp4bAUsjesB-yALOF4Z8uu_Had7a95ycrsfaw_CANgG0ptUrXSAXAVundo2u3KYZ1yfxLUblVJcwgzX4ZARNElmVpf2MiW-tnUbxTPhIjiZDAhU0tWdsEysA), [2](https://www.google.com/goto?url=CAEShwEB6zswFWtZRVNjUI8CaoDfdXPgpxpzNhvOgS2s-9G_lKbR0P_WRNJpUzbi0ppu2dDLbRSpHRoejf6b0dRi81TL4pUoLmUS9Qr15H_6WP-coj6LlrJui2Y_ukdrvJ6LFg5yDsaLWKp9jCOAxmdFB8rOuVfyhb5Jcz3PjIWnFQ3bLeRQNUuWKG0), [3](https://www.google.com/goto?url=CAESfQHrOzAVEZedUQmCP0HBlL0S2VFz3mpGsVETj_4r_CwQcRR9cW5W_KZ9rsoC5wN_YTVnEcvayGEPowOn_aASqOV-bcnKtszoNFRSCGy6Cn8PAeU-iYhN_yVAyPbd-DeQYuW6OY-9hAwFowE84WJcbifI9SoCmcH4eSMeov6d)]

Why choose Yes:

- **Faster Navigations:** It allows the application to serve static layout shells instantly while streaming dynamic data via React Suspense. [[1](https://www.google.com/goto?url=CAESfQHrOzAVEZedUQmCP0HBlL0S2VFz3mpGsVETj_4r_CwQcRR9cW5W_KZ9rsoC5wN_YTVnEcvayGEPowOn_aASqOV-bcnKtszoNFRSCGy6Cn8PAeU-iYhN_yVAyPbd-DeQYuW6OY-9hAwFowE84WJcbifI9SoCmcH4eSMeov6d), [2](https://www.google.com/goto?url=CAESaQHrOzAVNHdyoMBpoc0b2Enp4bAUsjesB-yALOF4Z8uu_Had7a95ycrsfaw_CANgG0ptUrXSAXAVundo2u3KYZ1yfxLUblVJcwgzX4ZARNElmVpf2MiW-tnUbxTPhIjiZDAhU0tWdsEysA)]
- **Simpler Data Fetching:** Data fetching is dynamic by default, and you can explicitly cache code blocks exactly where needed using the new `use cache` directive. [[1](https://www.google.com/goto?url=CAESaAHrOzAVPSNOWMjkbtNgBhTvNXQGsFF2XhP_MtskAjxEB9KoDZxuZPfpBZ2fDQ5YCzI9nt3eCpBMZJw47u6YO6yFd8rXV07tiKTgv_2ZXPUYbfdpMcYfHrStXn1MmWt-vk4NYurbwXy2), [2](https://www.google.com/goto?url=CAESfQHrOzAVEZedUQmCP0HBlL0S2VFz3mpGsVETj_4r_CwQcRR9cW5W_KZ9rsoC5wN_YTVnEcvayGEPowOn_aASqOV-bcnKtszoNFRSCGy6Cn8PAeU-iYhN_yVAyPbd-DeQYuW6OY-9hAwFowE84WJcbifI9SoCmcH4eSMeov6d)]
- **Future-Proofing:** Next.js plans to make Cache Components the strict default behavior in Next.js 17. Choosing it now avoids future migration work. [[1](https://www.google.com/goto?url=CAESfQHrOzAVEZedUQmCP0HBlL0S2VFz3mpGsVETj_4r_CwQcRR9cW5W_KZ9rsoC5wN_YTVnEcvayGEPowOn_aASqOV-bcnKtszoNFRSCGy6Cn8PAeU-iYhN_yVAyPbd-DeQYuW6OY-9hAwFowE84WJcbifI9SoCmcH4eSMeov6d)]

Select **Yes** and hit enter! What is the **final question or step** appearing in your console now? (It will likely ask about customizing an **import alias** or creating `AGENTS.md`). [[1](https://www.google.com/goto?url=CAEScAHrOzAV64To2AlY8HGE8SRraaWbbtM94ypHrPlJ8anAmPxoEzTkjrh4S5XLP6lfNTRl3RugVM0UlJJzSww_56F2DKBfjpr-4fg9eS6aaiAIAAgEQQprTRRgpcm-ilkvYIHPGuipzyanoAJRx5wG1Fs)]


Select **Yes** (App Router).

The **App Router** is the modern, standard way to build applications in Next.js. It gives you access to the latest features like Server Components, simpler routing using folders, and better performance out of the box.

Unless you are maintaining a legacy codebase that strictly requires the older "Pages Router," you should always choose the App Router for new projects.

What is the **next question** showing up on your screen? (It usually asks about customizing the **import alias** like `@/*`).


Choose **No** unless you are specifically looking to test experimental, cutting-edge features.

The **React Compiler** is a tool that automatically optimizes your code (handling `useMemo` and `useCallback` for you) to make your app run faster. However, it is still relatively new in the ecosystem.

Here is how you should decide:

- **Select "No" (Recommended):** If you are building a standard portfolio, learning Next.js, or want maximum stability with third-party libraries.
- **Select "Yes":** Only if you want to experiment with the absolute latest React 19 features and don't mind troubleshooting potential bugs with older npm packages.

Once you answer this, what is the **next question** appearing in your terminal? (It usually asks about **Tailwind CSS** or the **App Router** next).