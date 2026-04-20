
<div align="center">

```
 ██████╗██╗   ██╗██████╗     ██████╗ ███████╗██████╗  ██████╗ 
██╔════╝██║   ██║╚════██╗    ██╔══██╗██╔════╝██╔══██╗██╔═██╗
██║     ██║   ██║ █████╔╝    ██████╔╝█████╗  ██████╔╝██║   ██║
██║     ╚██╗ ██╔╝██╔═══╝     ██╔══██╗██╔══╝  ██╔═══╝ ██║   ██║
╚██████╗ ╚████╔╝ ███████╗    ██║  ██║███████╗██║      ╚██████╔╝
 ╚═════╝  ╚═══╝  ╚══════╝    ╚═╝  ╚═╝╚══════╝╚═╝       ╚═════╝ 
```

### Discord Components V2 — The Ultimate Reference for `discord.js` & `discord.py`

[![discord.js](https://img.shields.io/badge/discord.js-v14.17%2B-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.js.org)
[![discord.py](https://img.shields.io/badge/discord.py-v2.0%2B-5865F2?style=for-the-badge&logo=python&logoColor=white)](https://discordpy.readthedocs.io/en/latest/)
[![License](https://img.shields.io/badge/License-MIT-ff66b2?style=for-the-badge)](LICENSE)
[![Made by](https://img.shields.io/badge/Made_by-xhub.devs_-black?style=for-the-badge)](https://xhub.devs.im)

> **Unlock the full potential of Discord's modern UI.** This repository provides the most comprehensive guide to Components V2, featuring in-depth explanations, practical patterns, and ready-to-use code examples for both `discord.js` and `discord.py`.

</div>

---

## 🚀 Overview

Discord Components V2 (CV2) revolutionizes how bots interact with users by offering a rich, flexible, and visually stunning message building system. Moving beyond the limitations of traditional embeds, CV2 empowers developers to create dynamic and interactive experiences directly within Discord. This guide serves as your definitive resource for mastering CV2, from fundamental concepts to advanced patterns, ensuring your bots stand out with cutting-edge UI.

---

## 📖 Table of Contents

- [What is Components V2?](#-what-is-components-v2)
- [Why CV2 Over Classic Embeds?](#-why-cv2-over-classic-embeds)
- [Installation & Requirements](#-installation--requirements)
  - [discord.js](#discordjs)
  - [discord.py](#discordpy)
- [The Golden Rule: CV2 Message Structure](#-the-golden-rule-cv2-message-structure)
- [Builder Reference: A Dual-Language Guide](#-builder-reference-a-dual-language-guide)
  - [ContainerBuilder / ui.Container](#containerbuilder--uicontainer)
  - [TextDisplayBuilder / ui.TextDisplay](#textdisplaybuilder--uitextdisplay)
  - [SectionBuilder + ThumbnailBuilder / ui.Section + ui.Thumbnail](#sectionbuilder--thumbnailbuilder--uisection--uithumbnail)
  - [SeparatorBuilder / ui.Separator](#separatorbuilder--uiseparator)
  - [MediaGalleryBuilder / ui.MediaGallery](#mediagallerybuilder--uimediagallery)
  - [ActionRowBuilder / ui.ActionRow](#actionrowbuilder--uiactionrow)
  - [FileComponent / ui.File](#filecomponent--uifile)
- [Pattern Library: Real-World Examples](#-pattern-library-real-world-examples)
- [How to Send CV2 Messages](#-how-to-send-cv2-messages)
- [Markdown & Timestamps](#-markdown--timestamps)
- [Common Mistakes & Troubleshooting](#-common-mistakes--troubleshooting)
- [AI Agent Brain: Empowering Your AI Assistant](#-ai-agent-brain-empowering-your-ai-assistant)
- [File Structure](#-file-structure)
- [Contact](#-contact)

---

## 🧠 What is Components V2?

**Components V2 (CV2)** represents Discord's significant upgrade to message presentation, effectively replacing the older `embeds: [...]` system. Instead of static embed objects, CV2 introduces a dynamic approach where messages are constructed using **visual builder classes**. These builders are then passed within the `components: []` array for `discord.js` or as the `view` parameter for `discord.py`, always accompanied by a specific flag (`MessageFlags.IsComponentsV2` in `discord.js`) or a root `LayoutView` in `discord.py`.

This new system offers unparalleled control over message layout, enabling:

*   **Enhanced Layout Control**: Design complex, multi-element messages with precision.
*   **Inline Media**: Seamlessly integrate images and media galleries directly within your message content.
*   **Interactive Elements**: Embed buttons and dropdown menus within the same visual block for intuitive user interaction.
*   **Rich Markdown per Section**: Apply distinct markdown formatting to individual text blocks, allowing for highly structured and readable content.

---

## ⚡ Why CV2 Over Classic Embeds?

CV2 provides a superior and more flexible alternative to classic embeds. The table below highlights the key advantages:

| Feature | Classic Embed | Components V2 |
|:----------------------------|:-------------:|:-------------:|
| **Left Color Bar** | ✅ | ✅ `.setAccentColor()` (JS) / `accent_color` (Py) |
| **Inline Fields** | ✅ (3-column grid) | ✅ via multiple TextDisplay rows |
| **Image Inside Message** | ✅ (image/thumbnail only) | ✅ `MediaGalleryBuilder` (JS) / `ui.MediaGallery` (Py) (full-width, multiple images) |
| **Thumbnail Beside Text** | ✅ (fixed position) | ✅ `SectionBuilder` (JS) / `ui.Section` (Py) (flexible placement) |
| **Buttons in Same Block** | ❌ (separate ActionRow) | ✅ `.addActionRowComponents()` (JS) / `ui.ActionRow` (Py) on container |
| **Select Menu in Block** | ❌ (separate ActionRow) | ✅ `.addActionRowComponents()` (JS) / `ui.ActionRow` (Py) on container |
| **Markdown per Section** | ❌ (one description) | ✅ Each `TextDisplayBuilder` (JS) / `ui.TextDisplay` (Py) is independent |
| **Divider Lines** | ❌ | ✅ `SeparatorBuilder` (JS) / `ui.Separator` (Py) |
| **Loading → Edit Pattern** | ✅ | ✅ (same or easier implementation) |
| **Mix with `content:` text** | ✅ | ❌ CV2 is an all-in solution; cannot mix with traditional `content` |

---

## 📦 Installation & Requirements

To begin leveraging Discord Components V2, ensure you have the correct versions of your chosen library installed.

### discord.js

Install the `discord.js` library via npm:

```bash
npm install discord.js
```

> **Minimum Version:** `discord.js v14.17.0` or higher is required. CV2 builders were introduced in this release; older versions will not support these features and may cause errors.

Verify your installed version:

```bash
npm list discord.js
```

**Essential Imports:**

```js
const {
    MessageFlags,          // Crucial for marking CV2 messages
    ContainerBuilder,      // The primary wrapper for all CV2 content
    TextDisplayBuilder,    // For rendering rich markdown text
    SeparatorBuilder,      // To add visual dividers and spacing
    SeparatorSpacingSize,  // Defines spacing for separators (Small | Large)
    SectionBuilder,        // For side-by-side text and accessory layouts
    ThumbnailBuilder,      // Image thumbnail accessory for SectionBuilder
    MediaGalleryBuilder,   // For embedding full-width image galleries
    MediaGalleryItemBuilder, // Individual items within a MediaGallery
    ActionRowBuilder,      // To group interactive components (buttons, selects)
    StringSelectMenuBuilder,   // For creating dropdown menus
    StringSelectMenuOptionBuilder, // Options for StringSelectMenu
    ButtonBuilder,         // For creating interactive buttons
    ButtonStyle,           // Defines button appearance (Primary, Success, etc.)
    AttachmentBuilder,     // Necessary for uploading files with MediaGallery
} = require("discord.js");
```

### discord.py

For `discord.py`, you must install directly from the GitHub repository, as the PyPI version may not include the latest CV2 features:

```bash
pip install git+https://github.com/Rapptz/discord.py
```

**Essential Imports:**

```python
from discord import ui
import discord
# Specific components are accessed via ui.ComponentName (e.g., ui.LayoutView, ui.Container, ui.TextDisplay, ui.Button, etc.)
```

---

## 🔒 The Golden Rule: CV2 Message Structure

Adhering to this rule is paramount for successful CV2 implementation. Any CV2 message—whether a reply, send, or edit—**MUST** include the appropriate flag or view, and **CANNOT** be mixed with traditional embeds or `content` fields.

**For `discord.js`:**

```js
// ✅ Correct: Always include MessageFlags.IsComponentsV2 and the components array
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [myContainer]
});

// ❌ Wrong: Missing the crucial MessageFlags.IsComponentsV2 flag
await interaction.reply({
    components: [myContainer]
});

// ❌ Wrong: Mixing CV2 components with traditional embeds will result in an error
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [myContainer],
    embeds: [{ title: "something" }]  // ← Invalid, will error
});

// ❌ Wrong: Mixing CV2 components with a 'content' field will be ignored or error
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [myContainer],
    content: "hello"  // ← Invalid, will be ignored or error
});
```

**For `discord.py`:**

```python
# ✅ Correct: Always pass the LayoutView object to the 'view' parameter
await interaction.response.send_message(view=layout)

# ❌ Wrong: Mixing CV2 components with traditional embeds will result in an error
await interaction.response.send_message(view=layout, embeds=[...]) # ← Invalid, will error

# ❌ Wrong: Mixing CV2 components with a 'content' field will be ignored or error
await interaction.response.send_message(view=layout, content="hello") # ← Invalid, will be ignored or error
```

---

## 🏗️ Builder Reference: A Dual-Language Guide

This section provides a detailed breakdown of each core CV2 builder, showcasing its usage in both `discord.js` and `discord.py`.

### ContainerBuilder / ui.Container

The `ContainerBuilder` (JS) or `ui.Container` (Py) acts as the **root wrapper** for all CV2 content. Every CV2 message begins with an instance of this builder.

**`discord.js` Example:**
```js
const container = new ContainerBuilder()
    .setAccentColor(0x5865F2);   // Optional: adds a colored left bar
```

**`discord.py` Example:**
```python
layout = ui.LayoutView() # The top-level view for discord.py
container = ui.Container(accent_color=discord.Colour.blurple())
layout.add_item(container) # Add the container to the layout view
```

**Key Methods/Attributes:**

| Method/Attribute (`discord.js`) | Method/Attribute (`discord.py`) | Description |
|:--------------------------------|:--------------------------------|:------------|
| `.setAccentColor(int)` | `accent_color=discord.Colour` | Sets the left-side color bar. Accepts an integer (JS) or a `discord.Colour` object (Py). |
| `.addTextDisplayComponents(...builders)` | `.add_item(ui.TextDisplay)` | Adds one or more text display components. |
| `.addSectionComponents(...builders)` | `.add_item(ui.Section)` | Adds one or more section components, often with thumbnails. |
| `.addSeparatorComponents(...builders)` | `.add_item(ui.Separator)` | Inserts horizontal divider lines. |
| `.addMediaGalleryComponents(...builders)` | `.add_item(ui.MediaGallery)` | Embeds a block for image galleries. |
| `.addActionRowComponents(...builders)` | `.add_item(ui.ActionRow)` | Adds a row of interactive components (buttons, select menus). |

**Accent Color Conversion (JavaScript):**

```js
// Convert hex string to integer:
const color = parseInt("ff66b2", 16);          // Result: 16737970
const color = parseInt("#ff66b2".replace("#", ""), 16);

// Directly use hex literals:
.setAccentColor(0xff66b2)
.setAccentColor(0x5865F2)   // Discord blurple
.setAccentColor(0x2ecc71)   // Green
.setAccentColor(0xe74c3c)   // Red
.setAccentColor(0xf1c40f)   // Yellow
```

---

### TextDisplayBuilder / ui.TextDisplay

This builder is used to render **markdown-formatted text** within a container, making it one of the most frequently used components.

**`discord.js` Example:**
```js
new TextDisplayBuilder().setContent("Your **rich markdown** text here")
```

**`discord.py` Example:**
```python
ui.TextDisplay("Your **rich markdown** text here")
```

Multiple `TextDisplay` components can be stacked within a container, with each rendering as an independent text block:

**`discord.js` Stacking Example:**
```js
container.addTextDisplayComponents(
    new TextDisplayBuilder().setContent("## Section Title"),
    new TextDisplayBuilder().setContent("> A blockquote for emphasis"),
    new TextDisplayBuilder().setContent("A regular paragraph follows."),
)
```

**`discord.py` Stacking Example:**
```python
container.add_item(ui.TextDisplay("## Section Title"))
container.add_item(ui.TextDisplay("> A blockquote for emphasis"))
container.add_item(ui.TextDisplay("A regular paragraph follows."))
```

**Supported Markdown:**

```
**bold**                → **bold text**
*italic*                → *italic*
__underline__           → __underline__
~~strikethrough~~       → ~~strikethrough~~
`inline code`           → `monospace code`
> blockquote            → indented quote
<@userId>               → @UserMention
<#channelId>            → #channel-mention
<@&roleId>              → @RoleMention
<t:unix:F>              → Full timestamp (e.g., Monday, April 20, 2026 9:38 AM)
<t:unix:R>              → Relative timestamp (e.g., 3 hours ago)
<:name:emojiId>         → Custom Discord emoji
```

---

### SectionBuilder + ThumbnailBuilder / ui.Section + ui.Thumbnail

These builders are used together to render text content **alongside a thumbnail image**, ideal for profile cards, status panels, or any layout requiring a small image next to text.

**`discord.js` Example:**
```js
const thumbnail = new ThumbnailBuilder({
    media: { url: "https://cdn.discordapp.com/avatars/..." } // URL to the thumbnail image
});

const section = new SectionBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent("**Shadow**"),
        new TextDisplayBuilder().setContent("> Bot Developer"),
        new TextDisplayBuilder().setContent("> discord.js v14")
    )
    .setThumbnailAccessory(thumbnail); // Attach the thumbnail to the section

container.addSectionComponents(section);
```

**`discord.py` Example:**
```python
thumbnail = ui.Thumbnail(media={"url": "https://cdn.discordapp.com/avatars/..."}) # URL to the thumbnail image

section = ui.Section(
    ui.TextDisplay("**Shadow**"),
    ui.TextDisplay("> Bot Developer"),
    ui.TextDisplay("> discord.py v2")
)
section.set_thumbnail_accessory(thumbnail) # Attach the thumbnail to the section

container.add_item(section)
```

> ⚠️ **Important Note:** A `ThumbnailBuilder` (JS) or `ui.Thumbnail` (Py) **cannot** be added directly to a `ContainerBuilder` or `ui.Container`. It **must** be set as an accessory on a `SectionBuilder` (JS) or `ui.Section` (Py).

---

### SeparatorBuilder / ui.Separator

Use these builders to add **horizontal divider lines** or spacing between different sections of your CV2 message, improving readability and visual organization.

**`discord.js` Example:**
```js
const separator = new SeparatorBuilder()
    .setDivider(true)                          // Set to true to show the line, false for just spacing
    .setSpacing(SeparatorSpacingSize.Small);   // Options: .Small or .Large

container.addSeparatorComponents(separator);
```

**`discord.py` Example:**
```python
separator = ui.Separator(divider=True, spacing=ui.SeparatorSpacingSize.small) # Options: .small or .large

container.add_item(separator)
```

**Spacing Options:**

| Value (`discord.js`) | Value (`discord.py`) | Description |
|:---------------------|:---------------------|:------------|
| `SeparatorSpacingSize.Small` | `ui.SeparatorSpacingSize.small` | Provides a tighter gap, ideal for subtle separation. |
| `SeparatorSpacingSize.Large` | `ui.SeparatorSpacingSize.large` | Offers more breathing room, suitable for distinct section breaks. |

---

### MediaGalleryBuilder / ui.MediaGallery

These builders allow you to embed **full-width images or media galleries** directly within your message, offering a much more impactful visual experience compared to classic embed images.

**`discord.js` Example (Remote URL):**
```js
const gallery = new MediaGalleryBuilder()
    .addItems(
        new MediaGalleryItemBuilder().setURL("https://i.imgur.com/example.png")
    );

container.addMediaGalleryComponents(gallery);
```

**`discord.js` Example (Uploaded Attachment):**
```js
const buffer    = await generateCanvas(); // Example: generate an image buffer
const attachment = new AttachmentBuilder(buffer, { name: "banner.png" });

const gallery = new MediaGalleryBuilder()
    .addItems(
        new MediaGalleryItemBuilder().setURL("attachment://banner.png") // Reference the attachment by name
    );

container.addMediaGalleryComponents(gallery);

// When sending the message, include the files array:
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container],
    files: [attachment]         // ← Required for attachment:// URLs
});
```

**`discord.py` Example (Remote URL):**
```python
gallery = ui.MediaGallery()
gallery.add_item(media="https://i.imgur.com/example.png")

container.add_item(gallery)
```

**`discord.py` Example (Uploaded Attachment):**
```python
# Assuming 'file' is a discord.File object (e.g., from discord.File("path/to/image.png"))
gallery = ui.MediaGallery()
gallery.add_item(media="attachment://banner.png") # Reference the attachment by name

container.add_item(gallery)

# When sending the message, include the files array:
await interaction.response.send_message(
    view=layout,
    files=[file] # ← Required for attachment:// URLs
)
```

---

### ActionRowBuilder / ui.ActionRow

`ActionRow` components are essential for embedding **interactive elements** like buttons and dropdown menus directly within your CV2 message containers, allowing them to render as part of the same visual block.

**`discord.js` Example (Dropdown Menu):**
```js
const select = new StringSelectMenuBuilder()
    .setCustomId("my_menu")
    .setPlaceholder("Choose an option...")
    .addOptions(
        new StringSelectMenuOptionBuilder().setLabel("Option A").setValue("a"),
        new StringSelectMenuOptionBuilder().setLabel("Option B").setValue("b"),
    );

const row = new ActionRowBuilder().addComponents(select); // Add the select menu to an action row

container.addActionRowComponents(row); // Add the action row to the container
```

**`discord.js` Example (Buttons):**
```js
const btn1 = new ButtonBuilder().setCustomId("yes").setLabel("Confirm").setStyle(ButtonStyle.Success);
const btn2 = new ButtonBuilder().setCustomId("no").setLabel("Cancel").setStyle(ButtonStyle.Danger);

const row = new ActionRowBuilder().addComponents(btn1, btn2); // Add buttons to an action row

container.addActionRowComponents(row);
```

**`discord.py` Example (Dropdown Menu):**
```python
select = ui.StringSelect(
    custom_id="my_menu",
    placeholder="Choose an option...",
    options=[
        discord.SelectOption(label="Option A", value="a"),
        discord.SelectOption(label="Option B", value="b"),
    ]
)

row = ui.ActionRow(select) # Add the select menu to an action row

container.add_item(row) # Add the action row to the container
```

**`discord.py` Example (Buttons):**
```python
btn1 = ui.Button(custom_id="yes", label="Confirm", style=discord.ButtonStyle.success)
btn2 = ui.Button(custom_id="no", label="Cancel", style=discord.ButtonStyle.danger)

row = ui.ActionRow(btn1, btn2) # Add buttons to an action row

container.add_item(row)
```

**ButtonStyle Options:**

| Style (`discord.js`) | Style (`discord.py`) | Appearance | Description |
|:---------------------|:---------------------|:-----------|:------------|
| `ButtonStyle.Primary` | `discord.ButtonStyle.primary` | Blue | A prominent, general-purpose button. |
| `ButtonStyle.Secondary` | `discord.ButtonStyle.secondary` | Grey | A less prominent, secondary action button. |
| `ButtonStyle.Success` | `discord.ButtonStyle.success` | Green | Indicates a positive or successful action. |
| `ButtonStyle.Danger` | `discord.ButtonStyle.danger` | Red | Signifies a potentially destructive or negative action. |
| `ButtonStyle.Link` | `discord.ButtonStyle.link` | Grey with arrow | Creates a hyperlink button. Requires `.setURL()` (JS) or `url` (Py) instead of `custom_id`. |

---

### FileComponent / ui.File

These components facilitate the attachment and inline display of files within your CV2 messages.

**`discord.js` Usage Note:**

While `discord.js` doesn't have a direct `FileComponent` builder, file attachments are typically handled via `AttachmentBuilder` in conjunction with `MediaGalleryItemBuilder` by referencing `attachment://filename.ext` URLs within a `MediaGalleryBuilder`. See the `MediaGalleryBuilder` section for an example.

**`discord.py` Example:**
```python
# Assuming 'file_object' is a discord.File object (e.g., discord.File("path/to/document.pdf"))
file_component = ui.File(file=file_object)
container.add_item(file_component)
```

---

## 🎨 Pattern Library: Real-World Examples

Explore practical, copy-paste-ready code examples for common Discord bot UI patterns. Each pattern is implemented for both `discord.js` and `discord.py` to demonstrate cross-platform application.

Refer to the `examples/` directory for the full collection of runnable files:

*   **`examples/javascript/`**: `discord.js` implementations
*   **`examples/python/`**: `discord.py` implementations

---

## 📤 How to Send CV2 Messages

Sending CV2 messages requires specific syntax for each library. Below are common scenarios for replying, sending to channels, editing, and handling attachments.

**`discord.js` Sending Methods:**

```js
// ── Reply to a slash command interaction ───────────────────────────
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Reply to a prefix command (traditional message object) ─────────
await message.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Edit an existing message or reply ──────────────────────────────
await interaction.editReply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Send a new message to a specific channel ───────────────────────
await channel.send({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Send an ephemeral (only visible to sender) reply ───────────────
await interaction.reply({
    flags: MessageFlags.IsComponentsV2 | MessageFlags.Ephemeral,
    components: [container]
});

// ── Send a message with an uploaded file attachment ────────────────
// 'buffer' could be an image buffer, 'image.png' the desired filename
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container],
    files: [new AttachmentBuilder(buffer, { name: "image.png" })]
});

// ── Retrieve the sent message object (e.g., for latency calculation) ─
const msg = await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container],
    withResponse: true // Ensures the message object is returned
}).then(r => r.resource.message);
// Now 'msg' can be used to access properties like msg.createdTimestamp
```

**`discord.py` Sending Methods:**

```python
# ── Reply to a slash command interaction ───────────────────────────
await interaction.response.send_message(view=layout)

# ── Reply to a prefix command (traditional message object) ─────────
await message.reply(view=layout)

# ── Edit an existing message or reply ──────────────────────────────
await interaction.edit_original_response(view=layout)

# ── Send a new message to a specific channel ───────────────────────
await channel.send(view=layout)

# ── Send an ephemeral (only visible to sender) reply ───────────────
await interaction.response.send_message(view=layout, ephemeral=True)

# ── Send a message with an uploaded file attachment ────────────────
# 'file_object' is a discord.File object (e.g., discord.File("path/to/image.png"))
await interaction.response.send_message(
    view=layout,
    files=[file_object]
)

# ── Retrieve the sent message object (e.g., for latency calculation) ─
sent_message = await interaction.response.send_message(view=layout, wait=True)
# Now 'sent_message' can be used to access properties like sent_message.created_at
```

---

## 🕐 Markdown & Timestamps

Discord's markdown support within `TextDisplay` components is robust, including dynamic timestamps. Timestamps are always based on Unix seconds.

```js
// Generate a Unix timestamp in seconds
const ts = Math.floor(Date.now() / 1000);

// Timestamp formatting options:
`<t:${ts}:F>`  // → "Monday, April 20, 2026 9:38 AM"   (Full date/time)
`<t:${ts}:f>`  // → "April 20, 2026 9:38 AM"            (Short full date/time)
`<t:${ts}:D>`  // → "April 20, 2026"                    (Date only)
`<t:${ts}:d>`  // → "04/20/2026"                        (Short date)
`<t:${ts}:T>`  // → "9:38:00 AM"                        (Time only)
`<t:${ts}:t>`  // → "9:38 AM"                           (Short time)
`<t:${ts}:R>`  // → "3 minutes ago"                     (Relative time)

// Example: Using a Discord object's createdTimestamp (milliseconds to seconds):
const joinTs = Math.floor(member.joinedTimestamp / 1000);
`<t:${joinTs}:F>` // Displays the member's join date/time
```

---

## ❌ Common Mistakes & Troubleshooting

Avoid these common pitfalls to ensure smooth CV2 development:

| ❌ Common Mistake | ✅ Fix (`discord.js`) | ✅ Fix (`discord.py`) |
|:----------------------------------------|:----------------------------------------------------|:---------------------------------------------------------------------------------|
| **Missing CV2 Flag/View** | Add `flags: MessageFlags.IsComponentsV2` to every send/reply/edit call. | Pass `view=layout` to every send/reply/edit call. |
| **Mixing with `embeds: []`** | CV2 completely replaces embeds; **never** mix them. | CV2 completely replaces embeds; **never** mix them. |
| **Mixing with `content: "text"`** | All text content must be within `TextDisplayBuilder`. | All text content must be within `ui.TextDisplay`. |
| **Incorrect Accent Color Format** | Convert hex string to integer: `parseInt("5865F2", 16)`. | Use `discord.Colour.from_rgb(r, g, b)` or predefined `discord.Colour` objects. |
| **Direct `ThumbnailBuilder` Use** | `ThumbnailBuilder` must be an accessory of `SectionBuilder`. | `ui.Thumbnail` must be an accessory of `ui.Section`. |
| **`ActionRow` Outside Container** | Use `.addActionRowComponents()` on the `ContainerBuilder`. | Use `container.add_item(ui.ActionRow(...))` to add to the `ui.Container`. |
| **Missing Flag/View on Edit** | `flags: MessageFlags.IsComponentsV2` is required on `editReply` too. | `view=layout` is required on `edit_original_response` too. |
| **`StringSelectMenu` `maxValues`** | Default is 1; explicitly set `maxValues` for multi-select. | Default is 1; explicitly set `max_values` for multi-select. |
| **Outdated Library Version** | Upgrade: `npm install discord.js@latest`. | Install from GitHub: `pip install git+https://github.com/Rapptz/discord.py`. |
| **Multiple Root Containers** | One `ContainerBuilder` per message is cleanest, though multiple are technically allowed. | One `LayoutView` per message, containing one or more `ui.Container` instances, is the cleanest approach. |

---

## 🤖 AI Agent Brain: Empowering Your AI Assistant

The `CV2_BRAIN.md` file within this repository is a meticulously crafted knowledge document designed for seamless integration with any AI agent or large language model. By providing this file as part of an AI's context (e.g., in a system prompt or initial message), you can enable it to:

*   **Generate CV2 Patterns**: Create any CV2 message pattern from scratch with high accuracy.
*   **Optimal Builder Selection**: Choose the most appropriate builder for any given UI requirement.
*   **Automated Error Prevention**: Automatically avoid common CV2 implementation mistakes.
*   **Correct Syntax Generation**: Produce production-ready send/edit syntax for both `discord.js` and `discord.py`.

**Usage:**

1.  Open `CV2_BRAIN.md`.
2.  Copy the entire content of the file.
3.  Paste it as the system prompt or the first message to your AI assistant.
4.  Instruct the AI to build any Discord CV2 embed, specifying the desired library (JavaScript or Python).
5.  Observe as it generates correct, efficient, and ready-to-use code.

---

## 📁 File Structure

This repository is organized for clarity and ease of navigation:

```
discord-components-v2-guide/
│
├── README.md              ← You are here: The comprehensive guide and reference.
├── CV2_BRAIN.md           ← The AI agent knowledge file.
│
└── examples/
    ├── javascript/        ← Contains runnable code examples for discord.js.
    │   ├── 01-simple-block.js         ← Basic text container.
    │   ├── 02-colored-info.js         ← Accent colored block.
    │   ├── 03-section-thumbnail.js    ← Text with side-by-side image.
    │   ├── 04-loading-edit.js         ← Two-step loading and update reply.
    │   ├── 05-mod-log.js              ← Severity-colored moderation log.
    │   ├── 06-media-gallery.js        ← Full-width image gallery.
    │   ├── 07-select-menu.js          ← Dropdown menu inside container.
    │   ├── 08-buttons.js              ← Confirm/cancel buttons.
    │   ├── 09-multi-field.js          ← User/server info style layout.
    │   └── 10-full-layout.js          ← Complex layout: gallery + text + dropdown.
    │
    └── python/            ← Contains runnable code examples for discord.py.
        ├── 01-simple-block.py         ← Basic text container.
        ├── 02-colored-info.py         ← Accent colored block.
        ├── 03-section-thumbnail.py    ← Text with side-by-side image.
        ├── 04-loading-edit.py         ← Two-step loading and update reply.
        ├── 05-mod-log.py              ← Severity-colored moderation log.
        ├── 06-media-gallery.py        ← Full-width image gallery.
        ├── 07-select-menu.py          ← Dropdown menu inside container.
        ├── 08-buttons.py              ← Confirm/cancel buttons.
        ├── 09-multi-field.py          ← User/server info style layout.
        └── 10-full-layout.py          ← Complex layout: gallery + text + dropdown.
```

---

## 📡 Contact

This guide was meticulously crafted by **xhub.devs**, an entity dedicated to creating clear, comprehensive, and actionable technical documentation.

[![Portfolio](https://img.shields.io/badge/⚓_Portfolio-xhub.devs-ff66b2?style=for-the-badge)](https://xhub.devs.im)
[![GitHub](https://img.shields.io/badge/GitHub-xhub.devs-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/xhub.devs-ai)

---

<div align="center">

```
☠  "Not all treasure is silver and gold, mate."  ☠
         — xhub.devs | DEAD MEN TELL NO TALES
```

*If this guide proved valuable, consider leaving a ⭐ on the repository — it fuels further exploration and development.*

</div>
