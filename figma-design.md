Use the `mcp__plugin_figma_figma__generate_figma_design` tool to export the current screen/page to Figma.

## Steps

1. **Verify authentication** — call `mcp__plugin_figma_figma__whoami` to confirm the user is logged in to Figma. If not authenticated, stop and ask the user to connect via `/mcp` before continuing.

2. **Identify the URL** — check if there's a localhost dev server running or a static HTML file. If a static HTML file exists, serve it with `npx serve . --listen 3456` and use `http://localhost:3456/<filename>.html`. Confirm the URL returns 200 before proceeding.

3. **Call without outputMode first** to get the available plans and files:
   ```
   mcp__plugin_figma_figma__generate_figma_design({})
   ```

4. **Ask the user where to save**, presenting the options returned:
   - **New file** (`newFile`) → show the list of available teams/plans and ask which one
   - **Existing file** (`existingFile`) → show recent files and ask which one (or accept a pasted Figma URL)
   - **Clipboard** (`clipboard`) → no extra input needed, copies for manual pasting

5. **Call again with outputMode + URL** using the user's choice:
   ```
   mcp__plugin_figma_figma__generate_figma_design({
     outputMode: "newFile",
     fileName: "<descriptive name>",
     planKey: "<chosen plan key>",
     url: "<page url>"
   })
   ```

6. **Poll until complete** — use the returned `captureId`, calling every 5 seconds up to 10 times:
   ```
   mcp__plugin_figma_figma__generate_figma_design({ captureId: "<id>" })
   ```
   Report to the user when status is `completed` and share the Figma file link if available.
