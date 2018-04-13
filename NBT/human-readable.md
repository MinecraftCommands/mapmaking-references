# Entity data

blah


# Player data

`<player>.dat` files are used by servers to store the state of individual players. The format is also used within level.dat files to store the state of the singleplayer player, which overrides any `<player>.dat` files with the same name as the singleplayer player. These files are in NBT format.

![Compound][compound] The root tag. In [[Level format|level.dat]] files, this tag is called "Player".  
* All tags from [[Chunk format#Entity format|Entities]] except the `id`, `CustomName`, and `CustomNameVisible` tags.
* All tags from [[Chunk format#Mobs|Mobs]] except `HandItems`, `ArmorItems`, `DropChances`, `CanPickUpLoot`, `PersistenceRequired`, `Leashed`, and `Leash`.
* ![Integer][int] **DataVersion**: Version of the player NBT structure. Is increased with every new snapshot and release.
* ![Integer][int] **Dimension**: The dimension the player is in. -1 is [[the Nether]], 0 is [[the Overworld]], 1 is [[the End]]. Other values are interpreted as 0.
* ![Integer][int] **playerGameType**: The game mode of the player. 0 is [[Survival]], 1 is [[Creative]], 2 is [[Adventure]] and 3 is [[Spectator]].
* ![Integer][int] **Score**: The Score displayed upon death.
* ![Integer][int] **SelectedItemSlot**: The selected hotbar slot of the player.
* ![Compound][compound] **SelectedItem**: Data of the item currently being held by the player, excluding the [[#Inventory slot numbers|Slot]] tag.
* * See [[#Item structure|Item Format]].
* ![Integer][int] **SpawnX**: ''See below.''
* ![Integer][int] **SpawnY**: May not exist. The coordinates of the player's bed. These tags are only removed if the player attempts to respawn with no valid bed to spawn at at these coordinates. They are unaffected by breaking beds at these coordinates, and are unaffected by the player's death.
* ![Integer][int] **SpawnZ**: ''See above.''
* ![Byte][byte] **SpawnForced**: 1 or 0 (true/false) - True if the player should spawn at the above coordinates even if no bed can be found.
* ![Byte][byte] **Sleeping**: 1 or 0 (true/false) - true if the player was in a bed; has no effect on whether the player is in a bed when they log in.
* ![Short Integer][short] **SleepTimer**: The number of ticks the player had been in bed. No effect.
* ![Integer][int] **foodLevel**: The value of the hunger bar; 20 is full. See [[Hunger]].
* ![Float][float] **foodExhaustionLevel**: See [[Hunger]].
* ![Float][float] **foodSaturationLevel**: See [[Hunger]].
* ![Integer][int] **foodTickTimer**: See [[Hunger]].
* ![Integer][int] **XpLevel**: The level shown on the [[XP]] bar.
* ![Float][float] **XpP**: The progress/percent across the XP bar to the next level.
* ![Integer][int] **XpTotal**: The total amount of XP the player has collected over time; used for the Score upon death.
* ![Integer][int] **XpSeed**: The seed used for the next enchantment in [[enchantment table]]s.
* ![List][list] **Inventory**: Each compound tag in this list is an item in the player's inventory. (Note: when empty, list type may have [[NBT#As used in Minecraft|unexpected value]].)
* * ![Compound][compound] An item in the inventory, includes the [[#Inventory slot numbers|Slot]] tag.
* * * See [[#Item structure|Item Structure]] below.
* ![List][list] **EnderItems**: Each compound tag in this list is an item in the player's 27-slot ender chest inventory. (Note: when empty, list type may have [[NBT#As used in Minecraft|unexpected value]].)
* * ![Compound][compound] An item in the inventory, includes the [[#Inventory slot numbers|Slot]] tag - slots are numbered 0 to 26, inclusive.
* * * See [[#Item structure|Item Structure]] below.
* ![Compound][compound] **abilities**: The abilities this player has.
* * ![Float][float] **walkSpeed**: The walking speed, always 0.1.
* * ![Float][float] **flySpeed**: The flying speed, always 0.05.
* * ![Byte][byte] **mayfly**: 1 or 0 (true/false) - true if the player can fly.
* * ![Byte][byte] **flying**: 1 or 0 (true/false) - true if the player is currently flying.
* * ![Byte][byte] **invulnerable**: 1 or 0 (true/false) - true if the player is immune to all damage and harmful effects except for [[The Void**void]] damage. (damage caused by the {{cmd|kill** command is void damage)
* * ![Byte][byte] **mayBuild**: 1 or 0 (true/false) - true if the player can place and destroy blocks.
* * ![Byte][byte] **instabuild**: 1 or 0 (true/false) - true if the player can instantly destroy blocks.
* ![Compound][compound] **RootVehicle**: The root entity that the player is riding.
* * ![Long Integer][long] **AttachLeast**: The UUIDLeast of the entity the player is riding.
* * ![Long Integer][long] **AttachMost**: The UUIDMost of the entity the player is riding.
* * ![Compound][compound] **Entity**: The NBT data of the root vehicle.
* * * See [[Chunk format#Entity format|Entity Format]].
* ![Compound][compound] **ShoulderEntityLeft**: The entity that is on the player's left shoulder. Will always display as a parrot.
* * See [[Chunk format#Entity format|Entity Format]].
* ![Compound][compound] **ShoulderEntityRight**: The entity that is on the player's right shoulder. Will always display as a parrot.
* * See [[Chunk format#Entity format|Entity Format]].
* ![Byte][byte] **seenCredits**: 1 or 0 (true/false) - true if the player has traveled to the [[Overworld]] via an [[End portal]].
* ![Compound][compound] **recipeBook**: Contains a JSON object detailing recipes the player has unlocked.
* * See [[Recipe Book#Data values]].


# Item data

Items are used both in the player's inventory and Ender inventory, and in chest [[Chunk format#Block entity format|tile entities]], dropped item entities, furnace tile entities, brewing stand tile entities, and Villager trading recipes. Sometimes a Slot tag is used to specify the slot the item is in, such as with chests; other times there is no Slot tag, such as with dropped items.

![Compound][compound] The item's root tag. Some instances may give this tag a name, other times it is nameless because it is in a list.
* ![Byte][byte] **Count**: Number of [[item]]s stacked in this inventory slot. Any item can be stacked, including tools, armor, and vehicles. Range is -128 to 127. Values of 1 are not displayed in-game. Values below 1 are displayed in red.<ref group=note>A value of zero means the item will disappear when you try to use or drop it, but it can still be crafted with, or fired in the case of [[arrow]]s, as though it were a stack of 1.

A negative value is similar except that the item can be used by right-clicking, can be used until it wears out if it is a tool or bow (but will break if you use it as a melee weapon). This can let the count go below -128, but it will wrap around (by adding the nearest multiple of 256) when the server restarts or the single-player game is saved and reopened. The same applies if two negative stacks are combined. If Q is pressed while holding a negative stack, it will drop all at once and appear as a normal dropped item, but it will be impossible to pick up.

A stack of more than 64 may decrease if moved to or from a container, or the mouse pointer may drop only as many as normally fill a stack and continue to hold the remainder. A stack of ''N'' damaged tools used until "they" break will become ''N'' – 1 ''intact'' tools, but if the stack is split then both tools keep their damage.</ref>
* ![Byte][byte] **Slot**: May not exist. The inventory slot the item is in.
* ![Short Integer][short] **Damage**: The data value for this item. The name "Damage" comes from when only tools used this value, now many other items use this value for other purposes. For blocks, it is the 4-bit "block data" tag that determines a variant of the block. Defaults to 0.{{upcoming**until=1.13**
* ![String][string] **id**: [[Data values|Item/Block ID]] (This is a Short tag prior to 1.8.) If not specified, Minecraft changes the item to [[stone]] (setting ID to 1 and Damage to 0, and ignoring any existing Damage value) when loading the chunk or summoning the item .<!-- Tested using NBTExplorer in 1.8.2-pre1 -->
* ![Compound][compound] **tag**: Additional information about the item, discussed in the below sections. This tag is optional for most items.

## General Tags

Items with durability can be made unbreakable and will never lose any durability. Additionally, items can have specifications for Adventure mode to describe which blocks may be broken with them.

![Compound][compound] **tag**: The `tag` tag.
* ![Integer][int] **Damage**: The damage value for this item. Defaults to 0. {{upcoming**ver=1.13**
* ![Byte][byte] **Unbreakable**: 1 or 0 (true/false) - if true, the item doesn't lose durability when used.
* ![List][list] **CanDestroy**: The only blocks this item may break when used by a player in [[adventure]] mode.
* * ![String][string]: The block ID.

## Block Tags

Blocks can be given tags to specify what blocks they may be placed against in Adventure mode, and to specify what Tile Entity NBT tags to apply to them when placed.

![Compound][compound] **tag**: The `tag` tag.
* ![List][list] **CanPlaceOn**: Determines which blocks that blocks with this tag can be placed against in [[adventure]] mode.
* * ![String][string]: The block ID.
* ![Compound][compound] **BlockEntityTag**: [[Block entity]] NBT tags which are applied when this block is placed.<ref>Assuming the block is of a type that creates a block entity at all. If not, BlockEntityTag has no effect.</ref> Used to store data on [[banner]]s and [[shield]]s or on blocks obtained in [[creative]] by holding {{key**ctrl** (or {{key**cmd** on mac) and pressing {{control**pick block** on a block containing a block entity.
* * See [[Chunk format#Block entity format|Tile Entity Format]]. Excludes x, y, z, and id tags.

## Enchantments

There are two ways enchantments are associated with items; the first way is that the item is actually enchanted and the enchantment affects the behavior of the item, and the second way is that the item is an [[enchanted book]] which simply stores the enchantments without actually affecting the behavior of the item. There is also the `RepairCost` tag which tracks [[anvil]] usage for items, making them more costly with every use of the anvil.

![Compound][compound] **tag**: The `tag` tag.
* ![List][list] **ench**: Contains [[enchantment]]s on this item that affect the way the item works.
* * ![Compound][compound]: A single enchantment.
* * * ![Short Integer][short] **id**: ID of the enchantment.
* * * ![Short Integer][short] **lvl**: Level of the enchantment, where 1 is level 1.
* ![List][list] **StoredEnchantments**: Contains enchantments for [[enchanted book]]s.
* * ![Compound][compound]: A stored enchantment, identical structure to each enchantment in `ench`.
* ![Integer][int] **RepairCost**: Number of enchantment levels to add to the base level cost when repairing, combining, or renaming this item with an [[Anvil]].
`StoredEnchantments` tooltips will not be displayed if edited onto an item other than an enchanted book. Enchantments stored in `ench`, however, will always be displayed in the tooltip and will cause the item to glow, even if they cannot have any effect.

## Attribute Modifiers

All items can be given [[Attribute#Modifiers|Modifiers]] which affect various [[Attribute]]s of the player/mob which wears or holds them. Note that if an item has vanilla default AttributeModifiers, these will cease to exist if this tag is added (e.g. a Diamond Sword given an empty AttributeModifiers list will no longer provide a boost to damage). Also note that the default vanilla AttributeModifiers do not actually use this tag, and as such, it will not appear on a natural item.

![Compound][compound] **tag**: The `tag` tag.
* ![List][list] **AttributeModifiers**: Contains Attribute Modifiers on this item which modify Attributes of the wearer or holder (if the item is not in the hand or armor slots, it will have no effect).
* * ![Compound][compound]: A single Attribute Modifier.
* * * ![String][string] **AttributeName**: The name of the Attribute this Modifier is to act upon.
* * * ![String][string] **Name**: Name of the Modifier
* * * ![String][string] **Slot**: Slot the item must be in for the modifier to take effect. "mainhand", "offhand", "feet", "legs", "chest", or "head".
* * * ![Integer][int] **Operation**: Modifier Operation. See [[Attribute#Modifiers|Attribute Modifiers]] for info.
* * * ![Double][double] **Amount**: Amount of change from the modifier.
* * * ![Long Integer][long] **UUIDMost**: Uppermost bits of the modifier's UUID.
* * * ![Long Integer][long] **UUIDLeast**: Lowermost bits of the modifier's UUID.

## Potion Effects

[[Potion]]s, [[splash potion]]s, [[lingering potion]]s and [[tipped arrow]]s can have multiple, customized effects via the CustomPotionEffects tag. These effects are added to the default effect under the Potion tag if present. In addition the color can be overridden with the CustomPotionColor tag.

![Compound][compound] **tag**: The `tag` tag.
* ![List][list] **CustomPotionEffects**: The custom [[Potion effects]] this potion has.
* * ![Compound][compound] One of these for each effect.
* * * ![Byte][byte] **Id**: The ID of the effect.
* * * ![Byte][byte] **Amplifier**: The amplifier of the effect, with 0 being level 1.
* * * ![Integer][int] **Duration**: The duration of the effect in [[tick]]s.
* * * ![Byte][byte] **Ambient**: 1 or 0 (true/false) - whether or not this is an effect provided by a beacon and therefore should be less intrusive on the screen. This tag is optional and defaults to 0. Due to a bug, it has no effect on splash potions.
* * * ![Byte][byte] **ShowParticles**: 1 or 0 (true/false) - whether or not this effect produces particles. This tag is optional and defaults to 1. Due to a bug, it has no effect on splash potions.
* ![String][string] **Potion**: The name of the default [[Potion#Item data|potion effect]]. This name differs from the potion effect name. For example, the value for an "Instant Health II" potion is "minecraft:strong_healing".
* ![Integer][int] **CustomPotionColor**: The overrides the color of the potion, to be the prescribed color (same format as leather armor colors below).

## Display Properties

[[Leather armor]] can be colored, and all items can have custom display names and lore. Various tooltips can also be hidden.

![Compound][compound] **tag**: The `tag` tag.
* ![Compound][compound] **display**: Display properties.
* * ![Integer][int] **color**: The color of the leather armor. The tooltip will display "Dyed" if advanced tooltips are disabled or will otherwise display the hexadecimal color value. Color codes are calculated from the Red, Green and Blue components using this formula:<br>`<span style="color:red">Red</span>[[wikipedia:Logical shift|<<]]16 + <span style="color:green">Green</span><<8 + <span style="color:blue">Blue</span>`<ref group=note>For positive values larger than 0x00FFFFFF, the top byte is ignored. All negative values produce white.</ref>
* * ![String][string] **Name**: The name to display for an item{{upcoming**until=1.13**, or the JSON text component to use to display the item{{upcoming**ver=1.13**.
* * ![String][string] **LocName**: A localization string to translate (e.g. `gui.toTitle`).{{upcoming**until=1.13**
* * ![List][list] **Lore**: List of strings to display as lore for the item.<ref>{{reddit**10xlod/minecraft_snapshot_12w40a|c6hinjw**</ref>
* * * ![String][string] A line of text for the lore of an item.
* ![Integer][int]eger**HideFlags**: Bit field determining which parts of the tooltip to hide on an item. 1 for "ench", 2 for "AttributeModifiers", 4 for "Unbreakable", 8 for "CanDestroy", 16 for "CanPlaceOn", and 32 for various other information (including potion effects, "StoredEnchantments", written book "generation" and "author", "Explosion", "Fireworks", and map tooltips). For example, setting to 3 would hide both "ench" and "AttributeModifiers" tags, and setting to 63 would hide everything.

## Written Books

Both [[book and quill]] and [[written book]] use the `tag` tag to store information about the book. Only written books store the `title`, `author`, and `generation` of the book; `pages` is stored in both varieties.

{{:Written Book/DV

## Player Heads

[[Head]]s of the player variety can be associated with a specific username to take on the skin of that player when placed. The hand-held item is also updated with the new skin. Within this section, the "owner" of a head means the player whose head it is a copy of, and a player whose inventory contains a head is called the "holder".

![Compound][compound] **tag**: The `tag` tag.
* <s>![String][string] **SkullOwner**</s> <sup>(deprecated since [[1.8]])</sup>: The username of the player this is a skull of for ''pre-1.8''. Note that `SkullOwner` is for the item in inventories, whereas `ExtraType` is for skull blocks placed on the ground.<ref>{{reddit**zxn7u/its_apparently_my_cakeday_so_lets_cash_in_this**</ref>
* ![Compound][compound] **SkullOwner**: definition for the skull's owner. Note that `Owner` is used for skulls placed on the ground.
* * ![String][string] **Id**: UUID of owner. Optional. Used to update the other tags when the chunk loads or the holder logs in, in case the owner's name has changed.
* * ![String][string] **Name**: Username of owner. If missing or empty, the head will appear as a Steve head. Otherwise, used to store or retrieve the downloaded skin in the cache. Need not be a valid player name, but must not be all spaces.
* * ![Compound][compound] **Properties**
* * * ![List][list] **textures**
* * * * ![Compound][compound]: An individual texture.
* * * * * ![String][string] **Signature**: Optional.
* * * * * ![String][string] **Value**: A [[wikipedia:Base64**Base64]]-encoded [[wikipedia:JSON|JSON]] object.<ref>FVBico and Steven W.d.V.'s comments on {{bug|51003**. <!-- The example FVBico gives for himself decodes to: {"timestamp":1397247036968,"profileId":"79bf99e1621c4e91bd81a77a08b386ba","profileName":"FVbico","isPublic":true,"textures":{"SKIN":{"url":"http://textures.minecraft.net/texture/305c424e2d2858ddf68a7facef8c55b78c63bb2461b62c63b7d78da5a7134a"} --></ref>
* * * * * * ![Long Integer][long] **timestamp**: Optional: [[wikipedia:Unix time|Unix time]] in milliseconds.
* * * * * * ![String][string] **profileId**: Optional: Player UUID without hyphens.
* * * * * * ![String][string] **profileName**: Optional: Player name.
* * * * * * {{nbt**boolean|isPublic**: Optional.
* * * * * * ![Compound][compound] **textures**
* * * * * * * ![Compound][compound] **SKIN**
* * * * * * * * ![String][string] **url**: URL of a player skin on textures.minecraft.net.<ref group="note">{{bug**78491|Could be a URL on a non-Mojang site** until [[1.8.4]], but couldn't be a [[wikipedia:data URI scheme|data:]] or [[wikipedia:file URI scheme|file:]] URI.</ref>
* * * * * * * ![Compound][compound] **CAPE**: Optional.
* * * * * * * * ![String][string] **url**: URL of a player cape (64x32 PNG).

## Fireworks

[[Fireworks]] use the `tag` tag to store information about their effects.

![Compound][compound] **tag**: The `tag` tag.
* ![Compound][compound] **Explosion**: One of these may appear on a [[firework star]].
* * ![Byte][byte] **Flicker**: 1 or 0 (true/false) - true if this explosion will have the Twinkle effect (glowstone dust). May be absent.
* * ![Byte][byte] **Trail**: 1 or 0 (true/false) - true if this explosion will have the Trail effect (diamond). May be absent.
* * ![Byte][byte] **Type**: The shape of this firework's explosion. 0 = Small Ball, 1 = Large Ball, 2 = Star-shaped, 3 = Creeper-shaped, 4 = Burst. Other values will be named "Unknown Shape" and render as Small Ball.
* * ![Integer Array][int_array] **Colors**: Array of integer values corresponding to the primary colors of this firework's explosion. If custom color codes are used, the game will render it as "Custom" in the tooltip, but the proper color will be used in the explosion. Custom colors are integers in the same format as the `color` tag from [[#Display Properties|Display Properties]].
* * ![Integer Array][int_array] **FadeColors**: Array of integer values corresponding to the fading colors of this firework's explosion. Same handling of custom colors as Colors. May be absent.
* ![Compound][compound] **Fireworks**: One of these may appear on a [[firework rocket]].
* * ![Byte][byte] **Flight**: Indicates the flight duration of the firework (equals the amount of gunpowder used in crafting the rocket). While this value can be anything from -128 to 127, values of -2 and under almost never detonate at all.
* * ![List][list] **Explosions**: List of compounds representing each explosion this firework will cause.
* * * ![Compound][compound] Same format as 'Explosion' compound on a firework star, as described above.

## Armor stands and spawn eggs

[[Armor stand]]s and [[spawn egg]]s may contain potential entity data.

![Compound][compound] **tag**: The `tag` tag.
* ![Compound][compound] **EntityTag**: Stores entity data that is applied to the armor stand when placed or entity when spawned.
* * See [[Chunk format#Entity format|Entity Format]].

## Maps

[[Map]]s may be scaled.

![Compound][compound] **tag**: The `tag` tag.
* ![Integer][int] **map**: The data value for this map. {{upcoming**ver=1.13**
* ![Integer][int] **map_scale_direction**: Only internally used when scaling a map, after that directly removed: The amount to increase the current map scale by when crafting. Always 1.
* ![Byte][byte] **map_tracking_position**: Only internally used when scaling a map, after that directly removed: 1 or 0 (true/false) - whether or not player markers should be added and updated. Currently unused.
* ![List][list] **Decorations**: A list of optional icons to display on the map. Decorations that are removed or modified will not update until the world is reloaded.
* * ![Compound][compound] An individual decoration.
* * * ![String][string] **id**: An arbitrary unique string identifying the decoration.
* * * ![Byte][byte] **type**: The ID of the [[Map#Map_icons|map icon]] to display.
* * * ![Double][double] **x**: The world X position of the decoration.
* * * ![Double][double] **z**: The world Z position of the decoration.
* * * ![Double][double] **rot**: The rotation of the symbol, ranging from 0.0 to 360.0, measured clockwise. A rotation of 0 displays the icon upside-down compared to its appearance in the icon texture.
* ![Compound][compound] **display**: The `display` tag.
* * ![Integer][int] **MapColor**: The color of the markings on the item's texture.

[byte]: https://i.imgur.com/2aoHpEz.png
[short]: https://i.imgur.com/8XkW2Sx.png
[int]: https://i.imgur.com/7XOLcz5.png
[long]: https://i.imgur.com/Ggd2RDs.png
[float]: https://i.imgur.com/wFlmwPV.png
[double]: https://i.imgur.com/XpHmYpj.png
[byte_array]: https://i.imgur.com/44SPsYV.png
[string]: https://i.imgur.com/jBhTWQG.png
[list]: https://i.imgur.com/QLqiURo.png
[compound]: https://i.imgur.com/ydb97dG.png
[int_array]: https://i.imgur.com/wfmqZFo.png
[long_array]: https://i.imgur.com/wfmqZFo.png
