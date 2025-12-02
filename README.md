# Ninjin's Bounty System

A comprehensive bounty system mod for DayZ that allows players to place bounties on other players, with automatic rule-breaker detection, reward systems, and extensive admin controls.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [Admin Menu](#admin-menu)
- [Admin Commands](#admin-commands)
- [Requirements](#requirements)
- [How It Works](#how-it-works)

## 🎯 Overview

Ninjin's Bounty System adds a complete bounty hunting mechanic to DayZ servers. Players can place bounties on other players using bounty tokens, and hunters can claim rewards for eliminating or surviving bountied targets. The system includes automatic rule-breaker detection for PvE/PvP violations, territory integration, and a full admin interface.

## ✨ Features

### Core Features
- **Bounty Placement**: Players place bounties using bounty tokens (Gold, Red, or Silver) or whatever classname you use.
- **Bounty Board**: Interactive board where players can skip bounties and place new ones
- **Reward System**: Configurable rewards.
- **Rule Breaker Detection**: Automatic detection and punishment for PvE players attacking PvP players. And also PvE vs PvE (Using Ninjins-PvP-PvE)
- **Territory Integration**: Optional integration with Expansion Territory (pause/teleport bountied players)
- **SafeZone Integration**: Optional integration with Expansion SafeZone (teleport bountied players out)
- **Map Tracking**: Optional circle drawing on map showing approximate location of bountied players
- **Countdown Timer**: On-screen countdown widget showing remaining bounty time
- **Cooldown System**: Prevents players from being bountied too frequently
- **Blacklist System**: Prevent specific players from receiving bounties
- **Automated Bounty Placement**: Server can automatically place bounties at intervals

### Reward Features
- **Reward Options**: Players can claim either a reward chest (items) or currency rewards with their claim count.
- **Random Item Selection**: Configurable random item picking with spawn chances
- **Item Quantity Control**: Direct amount setting or percentage-based quantity (min/max)
- **Attachment Support**: Support for item attachments with individual configuration
- **Currency Rewards**: Support for multiple currency types (e.g., Expansion banknotes)
- **Container Options**: Configurable container types (SeaChest, WoodenCrate, etc. and custom ones but Openable and Closable are not supported)
- **Ruined Container Option**: Option to ruin containers after populating

### Admin Features
- **Admin Menu**: Full in-game admin interface for configuring all settings except rewards these need to bet configured in json.
- **Tabbed Interface**: Separate tabs for normal settings and notification configuration
- **Live Config Editing**: Edit and save configuration without server restart
- **Config Reload**: Reload all configuration files without restart
- **Player Management**: Clear bounties, clear cooldowns, apply bounties.
- **Notification Customization**: Full control over all notification messages, titles, and icons

## 📦 Installation

1. **Download** the mod from the workshop or release page
4. **Configure**: Edit configuration files in `Config/` folder (see [Configuration](#configuration))
5. **Set Admins**: Add admin GUIDs to `Config/Admins.json` (You need to add Admins and restart for first admin)
6. **Start Server**: The mod will create default configs on first run

### File Structure
```
Ninjins_Bounty_System/
├── Config/
│   ├── BountyConfig.json              # Main configuration
│   ├── BountySuccessRewardConfig.json # Reward items and currency
│   ├── Admins.json                    # Admin GUIDs
│   └── Blacklist.json                  # Blacklisted player GUIDs
└── Logs/                              # Server logs
```

## ⚙️ Configuration

### Main Configuration (`BountyConfig.json`)

#### Core System Settings
- `EnableBountySystem`: Enable/disable the entire system
- `BountyCooldownSeconds`: Cooldown period before a player can be bountied again (0 = no cooldown)
- `MaxBountiedPlayers`: Maximum simultaneous bounties (-1 = unlimited, 0 = disabled)
- `MinOnlinePlayersRequired`: Minimum online players for system to be active
- `DisableSelfBounty`: Prevent players from placing bounties on themselves
- `SkipBountyTokenRequired`: Tokens needed to skip/transfer a bounty
- `PlaceBountyTokenRequired`: Tokens needed to place a bounty
- `MinimumPlayerLifetimeSeconds`: Minimum playtime before a player can receive a bounty (prevents freshie bounties)

#### Rule Breaker Settings
- `EnableRuleBreakerHitThreshold`: Use hit count threshold system or not
- `EnablePvEToPvPRuleBreaker`: Enable rule breaker PvE to PvP
- `PvEToPvPInstantRuleBreakerHits`: 1 = instant rulebreaker on first PvE-to-PvP hit, 0 = use threshold
- `BountyRuleBreakerDurationSeconds`: Duration for rule breaker bounties
- `RuleBreakerHitThresholdTime`: Time window to count hits (seconds)
- `RuleBreakerHitThresholdWarningHits`: Hits required for warning
- `RuleBreakerHitThresholdBountyHits`: Hits required for bounty placement because of rulebreaking
- `ClearPendingRewardsOnRuleBreakerBounty`: Clear rewards when rule breaker bounty is applied

#### Territory & SafeZone Settings
- `TeleportOutOfOwnTerritory`: Teleport bountied players out of their territory (Only Expansion currently supported)
- `PauseBountyInTerritory`: Pause bounty timer while in own territory (prevents griefing) (Only Expansion currently supported)
- `ResumeBountyDistanceFromTerritory`: Distance from territory edge to resume timer (Only Expansion currently supported)
- `TeleportOutOfSafeZone`: Teleport bountied players out of safezones (Only for Expansion and Ninjins-PvP-PvE)
- `TeleportOutOfSafeZoneDistance`: Distance to teleport from safezone (Only for Expansion and Ninjins-PvP-PvE)

#### Map Settings
- `BountyEnableMapDrawing`: Enable circle drawing on map
- `BountyCircleRadius`: Maximum circle radius (meters)
- `BountyCircleMinRadius`: Minimum circle radius (meters)
- `BountyCircleReduceRadiusOverTime`: Circle shrinks as bounty expires
- `BountyMapRequestCooldownSeconds` : Circle increases as bounty expires (starts from MinRadius)
- `BountyCircleColor`: Circle color (ARGB format) use ARGB to INT converter: https://argb-int-calculator.netlify.app/
- `BountyMapUpdateIntervalSeconds`: How often to update map drawing

#### UI Settings
- `CountdownWidgetPositionX/Y`: Position offset for countdown widget beginning from top right
- `CountdownWidgetWidth/Height`: Size of countdown widget
- `CountdownWidgetBackgroundColor`: Background color (ARGB format) use ARGB to INT converter: https://argb-int-calculator.netlify.app
- `CountdownWidgetTextColor`: Text color (ARGB format) use ARGB to INT converter: https://argb-int-calculator.netlify.app

### Reward Configuration (`BountySuccessRewardConfig.json`)

#### Reward Selection
- `EnableRandomRewardPick`: Randomly pick items based on spawn chances
- `RewardItemPickCount`: Number of items to pick based on chance (0 = all items)
- `ContainerClassName`: Container type to spawn (e.g., "SeaChest", "WoodenCrate")
- `RuinedContainerAsReward`: Set container to ruined after populating

#### Reward Items
Each item can have:
- `ItemClassName`: Class name of the item
- `SpawnChance`: Percentage chance to spawn (0.0-100.0)
- `Amount`: Direct quantity/amount (if > 0, ignores QuantMin/QuantMax)
- `QuantMin/QuantMax`: Quantity percentage range (0.0-1.0)
- `HealthMin/HealthMax`: Health/durability percentage range (0.0-1.0)
- `Attachments`: Array of attachments with individual configuration

#### Currency Rewards
Each currency can have:
- `ClassName`: Currency class name (e.g., "expansionbanknotehryvnia")
- `SpawnChance`: Percentage chance to spawn (0.0-100.0)
- `Amount`: Amount/value of currency to give

**Note**: Currency rewards respect `EnableRandomRewardPick` and `RewardItemPickCount` settings, just like item rewards.

### Admin Configuration (`Admins.json`)
```json
{
  "AdminGUIDs": [
    "YOUR_ADMIN_GUID_HERE"
  ]
}
```

### Blacklist Configuration (`Blacklist.json`)
```json
{
  "BlacklistedGUIDs": [
    "PLAYER_GUID_TO_BLACKLIST"
  ]
}
```

## 🎮 Admin Menu

Access the admin menu by setting up a Hotkey in your DayZ Settings.



## 📋 Requirements

### Required Mods
- CF
- Dabs

### Optional Mods (for additional features)
- **NinjinsPvPPvE**: Required for PvE/PvP zone detection and rule breaker system
- **Expansion Basebuilding**: Required for territory integration features
- **Expansion Core**: Required for SafeZone integration features


## 🎲 How It Works

### Placing a Bounty
1. Player approaches the bounty board
2. Player selects a target from the list of online players
3. System checks if player has required tokens
4. System checks if target is eligible (not on cooldown, not in safezone, etc.)
5. Bounty is placed and broadcast to server (if enabled)
6. Bountied player receives notification

### Claiming Rewards
1. Player approaches the bounty board
2. Player clicks "Claim Reward Chest" to get items in a container
3. OR player clicks "Claim Money" to get currency rewards directly
4. System checks if player has pending rewards
5. Rewards are given and pending count is decremented

### Rule Breaker System
1. PvE player attacks a PvP player
2. System tracks hits within the configured time window
3. If threshold is reached (or instant mode is enabled), rule breaker bounty is applied and PvE Players can kill the bountied player but the bountied player cant kill PvE players.
4. Rule breaker receives notification and can be hunted by PvP players
5. PvP players can fight back against rule breakers and vice versa

### Territory Integration
- If `PauseBountyInTerritory` is enabled: Bounty timer pauses while player is in their own territory
- If `TeleportOutOfOwnTerritory` is enabled: Player is teleported out when bounty is applied
- Timer resumes when player leaves territory (beyond configured distance)

### Map Tracking
- If enabled, a circle is drawn on the map showing approximate location
- Circle can shrink or grow over time based on configuration
- Circle position has random offset to prevent exact location tracking
- Updates at configured intervals

## 📝 Notes

- **Bounty Tokens**: Three types available `Ninjins_Bounty_Token_` (Gold, Red, Silver) - configure which are accepted
- **Bounty Board** : Classname: `Ninjins_Bounty_Board_Static`.
- **Reward Selection**: Both items and currency respect the same random pick and count settings
- **Amount vs QuantMin/QuantMax**: If `Amount` is set (> 0), it overrides percentage-based quantity settings
- **Spawn Chances**: All items, attachments, and currencies support individual spawn chances
- **Config Reload**: Changes take effect immediately after reload (no server restart needed)
- **Logs**: Check `Logs/` folder for detailed server logs
- **Logs**: Are set in LoggingSettings to 4 which means they are by default off. 1 = Logs everything.


## 🙏 Credits

Created by Ninjin

---

**For support, bug reports, or feature requests, please visit the mod's workshop page, my Discord.**

