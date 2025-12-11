# Video Storage Structure Analysis

**Server:** `192.168.238.162` (App Server)
**Path:** `/storage/_Video/`
**Date:** 2025-12-11

---

## Table of Contents

- [Current Directory Structure](#current-directory-structure)
- [Content Categorization](#content-categorization)
- [Issues with Current Structure](#issues-with-current-structure)
- [Recommended Directory Structure](#recommended-directory-structure)
- [Structure Benefits](#structure-benefits)
- [Migration Mapping](#migration-mapping)
- [Implementation Notes](#implementation-notes)
- [Recommended File Naming](#recommended-file-naming)

---

## Current Directory Structure

```
/storage/_Video/
├── HD/ (High Definition - 1080p)
│   ├── ADS2/                          - HD Advertisements
│   ├── HD BUMPERS/                    - HD Channel bumpers/promos
│   ├── MOVIES TITREBI/                - HD Movie titles/intros
│   ├── RES/                           - Reserved/Resources
│   ├── RUS2/                          - Russian dubbed content (main)
│   ├── RUS2 2015/                     - Russian dubbed (2015)
│   ├── RUS2 2016/                     - Russian dubbed (2016)
│   ├── RUS2 2017/                     - Russian dubbed (2017)
│   ├── RUS2 2019/                     - Russian dubbed (2019)
│   ├── RUS2 2021/                     - Russian dubbed (2021)
│   ├── RUS2 2023/                     - Russian dubbed (2023)
│   ├── RUS2 2025/                     - Russian dubbed (2025)
│   ├── RUS2 2026/                     - Russian dubbed (2026)
│   └── TEST/                          - Test files
│
└── SD/ (Standard Definition - 720p or lower)
    ├── ADs/                           - SD Advertisements
    ├── ENG/                           - English content
    ├── ENG ANONSEBI/                  - English announcements/promos
    ├── ENG BUMP/                      - English bumpers
    ├── GEO/                           - Georgian content (programs)
    ├── GEO BUMP/                      - Georgian bumpers
    ├── GEO SHORT AXALI/               - Georgian short clips (new)
    ├── REKLAMA/                       - Advertising
    ├── RES/                           - Reserved/Resources
    ├── SHORTS 2020/                   - Short clips (2020)
    ├── SHORTS 2024/                   - Short clips (2024)
    └── TMP/                           - Temporary files
```

---

## Content Categorization

### Main Content (Programs/Movies)
- `HD/RUS2/` - Russian dubbed movies (HD)
- `HD/RUS2 YYYY/` - Russian dubbed by year
- `SD/GEO/` - Georgian programs
- `SD/ENG/` - English programs

### Supporting Content (Exclude from playlist)
- `*BUMP*` - Channel bumpers
- `*ANONSEBI` - Announcements/promos
- `ADs/`, `ADS2/`, `REKLAMA/` - Advertisements
- `SHORTS/` - Short clips
- `RES/` - Resources
- `TMP/` - Temporary
- `TEST/` - Test files

---

## Issues with Current Structure

| # | Issue |
|---|-------|
| 1 | ❌ No channel separation - all channels share same directories |
| 2 | ❌ Inconsistent naming (ADs vs ADS2, REKLAMA) |
| 3 | ❌ RUS2 years scattered across multiple folders |
| 4 | ❌ No structure for outsource companies (RUS2, NEO) |
| 5 | ❌ Mix of content types in same level |
| 6 | ❌ No clear separation between shared and channel-specific content |
| 7 | ❌ Hard to manage permissions per channel |
| 8 | ❌ Difficult to add new outsource providers |

---

## Recommended Directory Structure

```
/storage/_Video/
│
├── CHANNELS/                           # Channel-specific content
│   ├── channel-1/                      # Main channel
│   │   ├── HD/
│   │   │   ├── Movies/                 # HD Movies for this channel
│   │   │   ├── Series/                 # HD TV Series
│   │   │   └── Specials/               # Special programs
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Specials/
│   │
│   ├── channel-2/                      # Movie channel
│   │   ├── HD/
│   │   │   ├── Movies/
│   │   │   ├── Series/
│   │   │   └── Specials/
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Specials/
│   │
│   └── channel-3/                      # Hits channel
│       ├── HD/
│       │   ├── Movies/
│       │   ├── Series/
│       │   └── Specials/
│       └── SD/
│           ├── Movies/
│           ├── Series/
│           └── Specials/
│
├── COMMON/                             # Shared content across channels
│   ├── Ads/                            # Advertisements
│   │   ├── HD/
│   │   │   ├── 2024/
│   │   │   ├── 2025/
│   │   │   └── Active/                 # Currently active ads
│   │   └── SD/
│   │       ├── 2024/
│   │       ├── 2025/
│   │       └── Active/
│   │
│   ├── Bumpers/                        # Channel bumpers/idents
│   │   ├── channel-1/
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   ├── channel-2/
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   └── channel-3/
│   │       ├── HD/
│   │       └── SD/
│   │
│   ├── Promos/                         # Promotional content
│   │   ├── HD/
│   │   └── SD/
│   │
│   ├── Titles/                         # Movie titles/intros
│   │   ├── HD/
│   │   └── SD/
│   │
│   └── Shorts/                         # Short-form content
│       ├── HD/
│       └── SD/
│
├── PROVIDERS/                          # Outsource content providers
│   ├── RUS2/                           # Russian dubbing studio
│   │   ├── Incoming/                   # New deliveries
│   │   │   └── YYYY-MM-DD/            # By delivery date
│   │   ├── Processing/                 # Being processed/QC
│   │   ├── Ready/                      # Ready for air
│   │   │   ├── HD/
│   │   │   │   ├── 2024/
│   │   │   │   ├── 2025/
│   │   │   │   └── 2026/
│   │   │   └── SD/
│   │   │       ├── 2024/
│   │   │       ├── 2025/
│   │   │       └── 2026/
│   │   └── Archive/                    # Old/completed
│   │
│   ├── NEO/                            # NEO dubbing studio
│   │   ├── Incoming/
│   │   ├── Processing/
│   │   ├── Ready/
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   └── Archive/
│   │
│   └── CUSTOM/                         # Placeholder for future providers
│       ├── Incoming/
│       ├── Processing/
│       ├── Ready/
│       └── Archive/
│
├── LANGUAGES/                          # Content organized by language
│   ├── Georgian/                       # GEO content
│   │   ├── HD/
│   │   │   ├── Movies/
│   │   │   ├── Series/
│   │   │   └── Documentaries/
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Documentaries/
│   │
│   ├── English/                        # ENG content
│   │   ├── HD/
│   │   │   ├── Movies/
│   │   │   ├── Series/
│   │   │   └── Documentaries/
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Documentaries/
│   │
│   └── Russian/                        # RUS content
│       ├── HD/
│       │   ├── Movies/
│       │   ├── Series/
│       │   └── Documentaries/
│       └── SD/
│           ├── Movies/
│           ├── Series/
│           └── Documentaries/
│
├── LIBRARY/                            # Master content library
│   ├── Movies/
│   │   ├── HD/
│   │   │   ├── Action/
│   │   │   ├── Comedy/
│   │   │   ├── Drama/
│   │   │   └── [other genres]/
│   │   └── SD/
│   │       └── [same structure]/
│   │
│   ├── Series/
│   │   ├── HD/
│   │   └── SD/
│   │
│   └── Documentaries/
│       ├── HD/
│       └── SD/
│
├── WORK/                               # Working directories
│   ├── Incoming/                       # New uploads
│   ├── Processing/                     # Being processed
│   │   ├── Transcoding/
│   │   ├── QC/                         # Quality Control
│   │   └── Metadata/
│   ├── Ready/                          # Ready for playout
│   └── Temp/                           # Temporary files
│
└── ARCHIVE/                            # Archive/backup
    ├── 2023/
    ├── 2024/
    └── 2025/
```

---

## Structure Benefits

### ✅ Channel Separation
- Each channel has its own directory
- Easy to manage permissions per channel
- Clear ownership and responsibility

### ✅ Shared Resources
- Common content (ads, bumpers) in one place
- No duplication
- Centralized management

### ✅ Provider Management
- Clear workflow: **Incoming → Processing → Ready → Archive**
- Easy to add new providers
- Track delivery status

### ✅ Language Organization
- Content organized by language
- Easy to find content for specific audience
- Supports multilingual channels

### ✅ Flexibility
- `CUSTOM/` directories for future needs
- Scalable structure
- Easy to extend

### ✅ Workflow Support
- `WORK/` directory for production pipeline
- Clear processing states
- Quality control integration

---

## Migration Mapping

| Old Location | New Location |
|-------------|--------------|
| `HD/RUS2/` | `PROVIDERS/RUS2/Ready/HD/` |
| `HD/RUS2 2025/` | `PROVIDERS/RUS2/Ready/HD/2025/` |
| `SD/GEO/` | `LANGUAGES/Georgian/SD/Movies/` |
| `SD/ENG/` | `LANGUAGES/English/SD/Movies/` |
| `HD/ADS2/` | `COMMON/Ads/HD/Active/` |
| `SD/ADs/` | `COMMON/Ads/SD/Active/` |
| `HD/HD BUMPERS/` | `COMMON/Bumpers/[channel]/HD/` |
| `SD/GEO BUMP/` | `COMMON/Bumpers/[channel]/SD/` |
| `HD/MOVIES TITREBI/` | `COMMON/Titles/HD/` |
| `SD/SHORTS 2024/` | `COMMON/Shorts/SD/` |
| `SD/TMP/` | `WORK/Temp/` |
| `HD/TEST/` | `WORK/Temp/` |

---

## Implementation Notes

### 1. Phased Migration
- Migrate in phases to avoid disruption
- Start with new content, gradually move old
- Keep old structure until migration complete

### 2. Symlinks During Transition
- Create symlinks from old to new locations
- Maintain backward compatibility
- Remove after full migration

### 3. Automation
- Update playout system paths
- Update parser to handle new structure
- Automate content organization

### 4. Documentation
- Document file naming conventions
- Create content ingestion guidelines
- Train staff on new structure

### 5. Permissions
- Set appropriate permissions per directory
- Restrict provider access to their folders
- Audit access regularly

---

## Recommended File Naming

### Movies
**Format:** `[Title]_[Year]_[Language]_[Provider]_[FileCode].[ext]`

**Example:** `Godzilla_2014_RUS_RUS2_1049A.mov`

### Series
**Format:** `[Title]_S[NN]E[NN]_[Language]_[Provider]_[FileCode].[ext]`

**Example:** `Friends_S01E01_ENG_NEO_2345A.mov`

### Ads
**Format:** `[Brand]_[Campaign]_[Duration]_[Language]_[ValidUntil].[ext]`

**Example:** `Brand_Campaign_30sec_GEO_2025-12-31.mov`

### Bumpers
**Format:** `[Channel]_[Type]_[Duration]_[Language].[ext]`

**Example:** `channel-3_Ident_5sec_GEO.mov`

---

**Last Updated:** 2025-12-11
