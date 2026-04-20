# 🧠 CV2 EMBED BRAIN
### Discord Components V2 — Complete Agent Knowledge File
> Feed this entire file to any agent or AI assistant.
> After reading this, the agent knows EVERYTHING needed to build any CV2 embed for both `discord.js` and `discord.py`.

---

## WHAT IS COMPONENTS V2?

Components V2 (CV2) is Discord's **replacement for classic embeds** (the `embeds: [...]` array).
Instead of an embed object, you build a message from **visual builder classes** and send them in the `components: []` array (JS) or `view` parameter (Py).

CV2 gives you:
- Left-side colored accent bars (like embed color)
- Thumbnail accessories next to text
- Image galleries inline in messages
- Dropdown menus and buttons inside the same container
- Separators / dividers between sections
- Richer markdown formatting per text block

**Supported versions:**
- `discord.js v14.17.0` or higher.
- `discord.py` installed from GitHub (`pip install git+https://github.com/Rapptz/discord.py`).

---

## THE ONE LAW — NEVER BREAK THIS

```js
// ✅ EVERY CV2 message, reply, or edit MUST include this flag (discord.js):
{
  flags: MessageFlags.IsComponentsV2,
  components: [container]
}

// ✅ EVERY CV2 message, reply, or edit MUST include this view (discord.py):
view=layout

// ❌ Without this flag/view, Discord rejects or ignores the components.
// ❌ You CANNOT mix CV2 components with embeds: [] in the same message.
// ❌ You CANNOT mix CV2 components with content: "text" in the same message.
```

---

## IMPORTS — COPY THIS BLOCK AT THE TOP OF ANY FILE

**discord.js:**
```js
const {
    MessageFlags,          // Required — IsComponentsV2 flag lives here
    ContainerBuilder,      // Outer wrapper for all CV2 content
    TextDisplayBuilder,    // Text rows (markdown supported)
    SeparatorBuilder,      // Horizontal divider line
    SeparatorSpacingSize,  // Small | Large spacing for separator
    SectionBuilder,        // Side-by-side text + accessory layout
    ThumbnailBuilder,      // Image thumbnail accessory (for SectionBuilder)
    MediaGalleryBuilder,   // Image gallery block (full-width images)
    MediaGalleryItemBuilder, // Individual items inside a MediaGallery
    ActionRowBuilder,      // Row of interactive components
    StringSelectMenuBuilder,   // Dropdown menu
    StringSelectMenuOptionBuilder, // Option inside dropdown
    ButtonBuilder,         // Button component
    ButtonStyle,           // Primary | Secondary | Success | Danger | Link
    AttachmentBuilder,     // For uploading files with MediaGallery
} = require('discord.js');
```

**discord.py:**
```python
from discord import ui
import discord
# Specific components are accessed via ui.ComponentName
# e.g., ui.LayoutView, ui.Container, ui.TextDisplay, ui.Button, etc.
```

> Only import what you actually use. The lists above are the full CV2 toolkit for each library.

---

## THE COMPONENT TREE — UNDERSTAND THIS FIRST

```
LayoutView (discord.py) / components: [ContainerBuilder] (discord.js) ← ALWAYS the root.
│
├── ContainerBuilder (JS) / ui.Container (Py)             ← Outer wrapper for all CV2 content.
│   │
│   ├── .setAccentColor(int) (JS) / accent_color=discord.Colour (Py) ← Left-side color bar. Pass hex as integer (JS) or discord.Colour object (Py).
│   │
│   ├── .addTextDisplayComponents(...) (JS) / .add_item(ui.TextDisplay(...)) (Py) ← Markdown text blocks. Stack as many as needed.
│   │       new TextDisplayBuilder().setContent('markdown here') (JS)
│   │       ui.TextDisplay('markdown here') (Py)
│   │
│   ├── .addSectionComponents(...) (JS) / .add_item(ui.Section(...)) (Py) ← Text BESIDE a thumbnail image.
│   │       new SectionBuilder().setThumbnailAccessory(new ThumbnailBuilder(...)) (JS)
│   │       ui.Section().set_thumbnail_accessory(ui.Thumbnail(...)) (Py)
│   │
│   ├── .addSeparatorComponents(...) (JS) / .add_item(ui.Separator(...)) (Py) ← Horizontal divider line.
│   │       new SeparatorBuilder().setDivider(true).setSpacing(SeparatorSpacingSize.Small) (JS)
│   │       ui.Separator(divider=True, spacing=ui.SeparatorSpacingSize.small) (Py)
│   │
│   ├── .addMediaGalleryComponents(...) (JS) / .add_item(ui.MediaGallery(...)) (Py) ← Full-width image(s) inline.
│   │       new MediaGalleryBuilder().addItems(new MediaGalleryItemBuilder().setURL('https://...')) (JS)
│   │       ui.MediaGallery().add_item(media='https://...') (Py)
│   │
│   ├── .addActionRowComponents(...) (JS) / .add_item(ui.ActionRow(...)) (Py) ← Dropdowns or buttons inside container.
│   │       new ActionRowBuilder().addComponents(selectMenu | button) (JS)
│   │       ui.ActionRow(selectMenu | button) (Py)
│   │
│   └── .addFileComponents(...) (JS - via AttachmentBuilder) / .add_item(ui.File(...)) (Py) ← Inline file attachments.
```

---

## ACCENT COLOR — HOW TO SET IT

**discord.js:**
```js
// Config-driven (like this bot uses):
const color = parseInt(String(config.color || '#5865F2').replace('#', ''), 16);
container.setAccentColor(color);

// Hardcoded examples:
container.setAccentColor(0x2ecc71);   // green
container.setAccentColor(0xe74c3c);   // red
container.setAccentColor(0xf1c40f);   // yellow
container.setAccentColor(0x5865F2);   // Discord blurple

// Color map pattern (ModBotX LoggerService):
const COLOR_MAP = {
    WARN:    0xf1c40f,
    KICK:    0xe67e22,
    BAN:     0xe74c3c,
    UNBAN:   0x2ecc71,
    MUTE:    0xe67e22,
    UNMUTE:  0x2ecc71,
    MASSBAN: 0xc0392b,
    PURGE:   0x3498db,
};
container.setAccentColor(COLOR_MAP[action] ?? 0x2f3136);
```

**discord.py:**
```python
# Using discord.Colour objects:
container = ui.Container(accent_color=discord.Colour.blurple())
container = ui.Container(accent_color=discord.Colour.green())
container = ui.Container(accent_color=discord.Colour.red())

# From hex string to discord.Colour:
hex_color = "#ff66b2"
color_int = int(hex_color.replace("#", ""), 16)
container = ui.Container(accent_color=discord.Colour(color_int))

# Color map pattern:
COLOR_MAP = {
    "WARN":    discord.Colour(0xf1c40f),
    "KICK":    discord.Colour(0xe67e22),
    "BAN":     discord.Colour(0xe74c3c),
    "UNBAN":   discord.Colour(0x2ecc71),
    "MUTE":    discord.Colour(0xe67e22),
    "UNMUTE":  discord.Colour(0x2ecc71),
    "MASSBAN": discord.Colour(0xc0392b),
    "PURGE":   discord.Colour(0x3498db),
}
container = ui.Container(accent_color=COLOR_MAP.get(action, discord.Colour(0x2f3136)))
```

---

## HOW TO SEND / REPLY / EDIT

**discord.js:**
```js
// ── Slash command reply ──────────────────────────────────────────
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Prefix command reply ─────────────────────────────────────────
await message.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Edit an existing reply ───────────────────────────────────────
await interaction.editReply({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Send to a channel directly ───────────────────────────────────
await channel.send({
    flags: MessageFlags.IsComponentsV2,
    components: [container]
});

// ── Message with uploaded file attachment + CV2 ──────────────────
const attachment = new AttachmentBuilder(buffer, { name: 'image.png' });
await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container],
    files: [attachment]
});
// Then in MediaGallery: .setURL('attachment://image.png')

// ── withResponse: true  (get the message object back) ───────────
const sent = await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [loadingContainer],
    withResponse: true
}).then(r => r.resource.message);
// Now you can use sent.createdTimestamp etc.
```

**discord.py:**
```python
# ── Slash command reply ──────────────────────────────────────────
await interaction.response.send_message(view=layout)

# ── Prefix command reply ─────────────────────────────────────────
await message.reply(view=layout)

# ── Edit an existing reply ───────────────────────────────────────
await interaction.edit_original_response(view=layout)

# ── Send to a channel directly ───────────────────────────────────
await channel.send(view=layout)

# ── Message with uploaded file attachment + CV2 ──────────────────
# Assuming 'file' is a discord.File object
await interaction.response.send_message(
    view=layout,
    files=[file]
)
# Then in MediaGallery: media="attachment://image.png"

# ── wait=True (get the message object back) ───────────────────
sent_message = await interaction.response.send_message(view=loading_layout, wait=True)
# Now you can use sent_message.created_at etc.
```

---

## PATTERN LIBRARY — COPY PASTE READY

### ① Simple Text Block
*Errors, confirmations, one-liner replies.*

**discord.js:**
```js
const block = new ContainerBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(`✅ Done.`)
    );

await context.reply({ flags: MessageFlags.IsComponentsV2, components: [block] });
```

**discord.py:**
```python
layout = ui.LayoutView()
container = ui.Container()
container.add_item(ui.TextDisplay("✅ Done."))
layout.add_item(container)

await context.reply(view=layout)
```

---

### ② Colored Info Block
*Stats, profiles, info commands.*

**discord.js:**
```js
const block = new ContainerBuilder()
    .setAccentColor(0x5865F2)
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(
            ['ℹ️ **Server Info**', '> **Members:** 1,234', '> **Boosts:** 3'].join('\n')
        )
    );
```

**discord.py:**
```python
layout = ui.LayoutView()
container = ui.Container(accent_color=discord.Colour.blurple())
container.add_item(ui.TextDisplay(
    "ℹ️ **Server Info**\n> **Members:** 1,234\n> **Channels:** 42\n> **Created:** <t:{Math.floor(guild.createdTimestamp / 1000)}:F>"
))
layout.add_item(container)
```

---

### ③ Section with Thumbnail
*Ping command, user profile, anything needing a side image.*

**discord.js:**
```js
const thumbnail = new ThumbnailBuilder({
    media: { url: client.user.displayAvatarURL({ extension: 'png', size: 256 }) }
});

const section = new SectionBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(`🌐 **Network Status**`),
        new TextDisplayBuilder().setContent(`> ✅ **Latency:** \`${latency}ms\``),
        new TextDisplayBuilder().setContent(`> 🔗 **API:** \`${apiPing}ms\``)
    )
    .setThumbnailAccessory(thumbnail);

const separator = new SeparatorBuilder()
    .setDivider(true)
    .setSpacing(SeparatorSpacingSize.Small);

const footer = new TextDisplayBuilder()
    .setContent(`*Cluster 0 | Shard 0*`);

const container = new ContainerBuilder()
    .addSectionComponents(section)
    .addSeparatorComponents(separator)
    .addTextDisplayComponents(footer);

await interaction.editReply({ flags: MessageFlags.IsComponentsV2, components: [container] });
```

**discord.py:**
```python
thumbnail = ui.Thumbnail(media={"url": client.user.display_avatar.url})

section = ui.Section(
    ui.TextDisplay("🌐 **Network Status**"),
    ui.TextDisplay(f"> ✅ **Latency:** `{latency}ms`"),
    ui.TextDisplay(f"> 🔗 **API:** `{api_ping}ms`")
)
section.set_thumbnail_accessory(thumbnail)

separator = ui.Separator(divider=True, spacing=ui.SeparatorSpacingSize.small)

footer = ui.TextDisplay("*Cluster 0 | Shard 0*")

layout = ui.LayoutView()
container = ui.Container()
container.add_item(section)
container.add_item(separator)
container.add_item(footer)
layout.add_item(container)

await interaction.edit_original_response(view=layout)
```

---

### ④ Moderation Action Block
*Ban, kick, warn, timeout — any mod command reply.*

**discord.js:**
```js
const block = new ContainerBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(
            `🔨 **${targetUser.tag}** has been banned. \`Case #${caseId}\`\n> **Reason:** ${reason}`
        )
    );

await context.reply({ flags: MessageFlags.IsComponentsV2, components: [block] });
```

**discord.py:**
```python
layout = ui.LayoutView()
container = ui.Container()
container.add_item(ui.TextDisplay(
    f"🔨 **{target_user.name}** has been banned. `Case #{case_id}`\n> **Reason:** {reason}"
))
layout.add_item(container)

await context.reply(view=layout)
```

---

### ⑤ Log Channel Block
*Send to a mod-log channel with severity color.*

**discord.js:**
```js
const COLOR_MAP = { BAN: 0xe74c3c, KICK: 0xe67e22, WARN: 0xf1c40f, UNBAN: 0x2ecc71 };

const block = new ContainerBuilder()
    .setAccentColor(COLOR_MAP[action] ?? 0x2f3136)
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(
            [
                `📁 **Case #${caseId} | ${action}**`,
                `> **Target:** <@${targetId}>`,
                `> **Moderator:** <@${modId}>`,
                `> **Reason:** ${reason}`,
                `> **Time:** <t:${Math.floor(Date.now() / 1000)}:F>`,
            ].join('\n')
        )
    );

await logChannel.send({ flags: MessageFlags.IsComponentsV2, components: [block] });
```

**discord.py:**
```python
COLOR_MAP = {
    "BAN": discord.Colour(0xe74c3c),
    "KICK": discord.Colour(0xe67e22),
    "WARN": discord.Colour(0xf1c40f),
    "UNBAN": discord.Colour(0x2ecc71)
}

layout = ui.LayoutView()
container = ui.Container(accent_color=COLOR_MAP.get(action, discord.Colour(0x2f3136)))
container.add_item(ui.TextDisplay(
    f"📁 **Case #{case_id} | {action}**\n" \
    f"> **Target:** <@{target_id}>\n" \
    f"> **Moderator:** <@{mod_id}>\n" \
    f"> **Reason:** {reason}\n" \
    f"> **Time:** <t:{int(discord.utils.utcnow().timestamp())}:F>"
))
layout.add_item(container)

await log_channel.send(view=layout)
```

---

### ⑥ Loading → Edit Pattern
*Send a placeholder immediately, then update with the real result.*

**discord.js:**
```js
// Step 1: instant loading reply
const loadingBlock = new ContainerBuilder()
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(`⏳ **Calculating...**`)
    );

const sentMsg = await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [loadingBlock],
    withResponse: true
}).then(r => r.resource.message);

// Step 2: do your async work, then edit
const latency = sentMsg.createdTimestamp - interaction.createdTimestamp;

const resultBlock = new ContainerBuilder()
    .setAccentColor(0x2ecc71)
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent(`✅ **Pong!** \`${latency}ms\``)
    );

await interaction.editReply({ flags: MessageFlags.IsComponentsV2, components: [resultBlock] });
```

**discord.py:**
```python
# Step 1: instant loading reply
loading_layout = ui.LayoutView()
loading_container = ui.Container()
loading_container.add_item(ui.TextDisplay("⏳ **Calculating...**"))
loading_layout.add_item(loading_container)

sent_message = await interaction.response.send_message(view=loading_layout, wait=True)

# Step 2: do your async work, then edit
latency = int((discord.utils.utcnow() - interaction.created_at).total_seconds() * 1000)

result_layout = ui.LayoutView()
result_container = ui.Container(accent_color=discord.Colour.green())
result_container.add_item(ui.TextDisplay(f"✅ **Pong!** `{latency}ms`"))
result_layout.add_item(result_container)

await interaction.edit_original_response(view=result_layout)
```

---

### ⑦ MediaGallery — Inline Image
*Embed a canvas-generated image or any URL as a full-width gallery.*

**discord.js:**
```js
const gallery = new MediaGalleryBuilder()
    .addItems(
        new MediaGalleryItemBuilder().setURL('https://i.imgur.com/example.png')
    );

const container = new ContainerBuilder().addMediaGalleryComponents(gallery);

await interaction.reply({ flags: MessageFlags.IsComponentsV2, components: [container] });
```

**discord.py:**
```python
layout = ui.LayoutView()
container = ui.Container()
gallery = ui.MediaGallery()
gallery.add_item(media="https://i.imgur.com/example.png")
container.add_item(gallery)
layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

### ⑧ Select Menu
*Interactive dropdown menu.*

**discord.js:**
```js
const select = new StringSelectMenuBuilder()
    .setCustomId('my_menu')
    .setPlaceholder('Choose an option...')
    .addOptions(
        new StringSelectMenuOptionBuilder().setLabel('Option A').setValue('a'),
        new StringSelectMenuOptionBuilder().setLabel('Option B').setValue('b'),
    );

const row = new ActionRowBuilder().addComponents(select);
const container = new ContainerBuilder().addActionRowComponents(row);

await interaction.reply({ flags: MessageFlags.IsComponentsV2, components: [container] });
```

**discord.py:**
```python
select = ui.StringSelect(
    custom_id="my_menu",
    placeholder="Choose an option...",
    options=[
        discord.SelectOption(label="Option A", value="a"),
        discord.SelectOption(label="Option B", value="b"),
    ]
)

row = ui.ActionRow(select)

layout = ui.LayoutView()
container = ui.Container()
container.add_item(row)
layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

### ⑨ Buttons
*Confirm/cancel buttons.*

**discord.js:**
```js
const btn1 = new ButtonBuilder().setCustomId('yes').setLabel('Confirm').setStyle(ButtonStyle.Success);
const btn2 = new ButtonBuilder().setCustomId('no').setLabel('Cancel').setStyle(ButtonStyle.Danger);

const row = new ActionRowBuilder().addComponents(btn1, btn2);
const container = new ContainerBuilder().addActionRowComponents(row);

await interaction.reply({ flags: MessageFlags.IsComponentsV2, components: [container] });
```

**discord.py:**
```python
btn1 = ui.Button(custom_id="yes", label="Confirm", style=discord.ButtonStyle.success)
btn2 = ui.Button(custom_id="no", label="Cancel", style=discord.ButtonStyle.danger)

row = ui.ActionRow(btn1, btn2)

layout = ui.LayoutView()
container = ui.Container()
container.add_item(row)
layout.add_item(container)

await interaction.response.send_message(view=layout)
```

---

### ⑩ Full Layout (Image + Text + Dropdown)
*Gallery, text, and interactive components combined.*

**discord.js:**
```js
const gallery = new MediaGalleryBuilder()
    .addItems(new MediaGalleryItemBuilder().setURL('attachment://banner.png'));

const select = new StringSelectMenuBuilder()
    .setCustomId('nav')
    .setPlaceholder('Navigate...')
    .addOptions(
        new StringSelectMenuOptionBuilder().setLabel('Home').setValue('home'),
        new StringSelectMenuOptionBuilder().setLabel('Commands').setValue('commands'),
    );

const container = new ContainerBuilder()
    .setAccentColor(0xff66b2)
    .addMediaGalleryComponents(gallery)
    .addTextDisplayComponents(
        new TextDisplayBuilder().setContent('## 📚 Help Menu'),
        new TextDisplayBuilder().setContent('Pick a category from the menu below.')
    )
    .addActionRowComponents(new ActionRowBuilder().addComponents(select));

await interaction.reply({
    flags: MessageFlags.IsComponentsV2,
    components: [container],
    files: [new AttachmentBuilder(buffer, { name: 'banner.png' })] // if using attachment://
});
```

**discord.py:**
```python
# Assuming 'banner_file' is a discord.File object if using attachment://
gallery = ui.MediaGallery()
gallery.add_item(media="attachment://banner.png")

select = ui.StringSelect(
    custom_id="nav",
    placeholder="Navigate...",
    options=[
        discord.SelectOption(label="Home", value="home"),
        discord.SelectOption(label="Commands", value="commands"),
    ]
)

row = ui.ActionRow(select)

layout = ui.LayoutView()
container = ui.Container(accent_color=discord.Colour(0xff66b2))
container.add_item(gallery)
container.add_item(ui.TextDisplay("## 📚 Help Menu"))
container.add_item(ui.TextDisplay("Pick a category from the menu below."))
container.add_item(row)
layout.add_item(container)

await interaction.response.send_message(
    view=layout,
    files=[banner_file] # if using attachment://
)
```

---

## MINIMAL WORKING COMMAND TEMPLATE

**discord.js:**
```js
// Paste this as the skeleton for any new CV2 command:

const {
    SlashCommandBuilder,
    MessageFlags,
    ContainerBuilder,
    TextDisplayBuilder,
} = require('discord.js');

module.exports = {
    data: new SlashCommandBuilder()
        .setName('example')
        .setDescription('An example CV2 command.'),

    async execute(interaction) {
        const block = new ContainerBuilder()
            .setAccentColor(0x5865F2)
            .addTextDisplayComponents(
                new TextDisplayBuilder().setContent(
                    [
                        `ℹ️ **Example Command**`,
                        `> **User:** ${interaction.user.tag}`,
                        `> **Server:** ${interaction.guild.name}`,
                    ].join('\n')
                )
            );

        await interaction.reply({
            flags: MessageFlags.IsComponentsV2,
            components: [block],
        });
    },
};
```

**discord.py:**
```python
# Paste this as the skeleton for any new CV2 command:

import discord
from discord import app_commands, ui

class MyBot(discord.Client):
    def __init__(self):
        super().__init__(intents=discord.Intents.default())
        self.tree = app_commands.CommandTree(self)

    async def on_ready(self):
        print(f'Logged in as {self.user} (ID: {self.user.id})')
        await self.tree.sync()

    @app_commands.command(name="example", description="An example CV2 command.")
    async def example_command(self, interaction: discord.Interaction):
        layout = ui.LayoutView()
        container = ui.Container(accent_color=discord.Colour.blurple())
        container.add_item(ui.TextDisplay(
            f"ℹ️ **Example Command**\n" \
            f"> **User:** {interaction.user.name}\n" \
            f"> **Server:** {interaction.guild.name}"
        ))
        layout.add_item(container)

        await interaction.response.send_message(view=layout)

# Example usage:
bot = MyBot()
bot.run("YOUR_BOT_TOKEN")
```

---

*End of CV2 Brain. An agent that reads this file fully can build any Discord Components V2 embed from scratch for both `discord.js` and `discord.py`.*
