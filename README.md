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
- [Storage Architecture Matrix](#storage-architecture-matrix)
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
- `HD/RUS2/` - Russian dubbed movies (HD) - Provider: RUS2
- `HD/RUS2 YYYY/` - Russian dubbed by year - Provider: RUS2
- `SD/GEO/` - Georgian programs - for magti-my channel
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

**Note:** Playlists will reference files from `PROVIDERS/` section. Channel-specific directories can contain symlinks to provider content or channel-exclusive material.

```
/storage/_Video/
│
├── CHANNELS/                           # Channel-specific content
│   ├── magti-my/                       # Magti My (Main channel - Georgian content)
│   │   ├── HD/
│   │   │   ├── Movies/                 # HD Movies for this channel
│   │   │   ├── Series/                 # HD TV Series
│   │   │   └── Specials/               # Special programs
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Specials/
│   │
│   ├── magti-kino/                     # Magti Kino (Movie channel)
│   │   ├── HD/
│   │   │   ├── Movies/
│   │   │   ├── Series/
│   │   │   └── Specials/
│   │   └── SD/
│   │       ├── Movies/
│   │       ├── Series/
│   │       └── Specials/
│   │
│   └── magti-hit/                      # Magti Hit (Hits channel)
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
│   │   ├── magti-my/
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   ├── magti-kino/
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   └── magti-hit/
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
├── PROVIDERS/                          # Outsource content providers (MAIN PLAYLIST SOURCE)
│   ├── RUS2/                           # Russian dubbing studio
│   │   ├── Incoming/                   # New deliveries
│   │   │   └── YYYY-MM-DD/            # By delivery date
│   │   ├── Processing/                 # Being processed/QC
│   │   ├── Ready/                      # Ready for air (PLAYLIST SOURCE)
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
│   │   ├── Ready/                      # Ready for air (PLAYLIST SOURCE)
│   │   │   ├── HD/
│   │   │   └── SD/
│   │   └── Archive/
│   │
│   ├── GEO/                            # Georgian content provider (magti-my)
│   │   ├── Incoming/
│   │   ├── Processing/
│   │   ├── Ready/                      # Ready for air (PLAYLIST SOURCE)
│   │   │   ├── HD/
│   │   │   │   ├── Movies/
│   │   │   │   ├── Series/
│   │   │   │   └── Documentaries/
│   │   │   └── SD/
│   │   │       ├── Movies/
│   │   │       ├── Series/
│   │   │       └── Documentaries/
│   │   └── Archive/
│   │
│   └── CUSTOM/                         # Placeholder for future providers
│       ├── Incoming/
│       ├── Processing/
│       ├── Ready/
│       └── Archive/
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

## Storage Architecture Matrix

| Directory | Mount Point | Server | Access | Purpose |
|-----------|-------------|--------|--------|---------|
| **CHANNELS/** | Local | `192.168.238.162` (App Server) | Internal | Channel-specific content, symlinks to PROVIDERS |
| **COMMON/** | Local | `192.168.238.162` (App Server) | Internal | Shared resources (Ads, Bumpers, Promos, Titles, Shorts) |
| **PROVIDERS/** | **Remote Mount** | **Public-facing server** | **Public Internet** | **Content upload/download for external providers (RUS2, NEO, GEO). Playlist source.** |
| **LIBRARY/** | Local | `192.168.238.162` (App Server) | Internal | Master content library organized by genre |
| **WORK/** | Local | `192.168.238.162` (App Server) | Internal | Working directories for content processing |
| **ARCHIVE/** | **Remote Mount** | **Archive Server** | **Internal** | **Long-term storage and backup** |

### Mount Details

#### PROVIDERS Directory
- **Mount Type:** Remote filesystem (NFS/SMBFS/FTP)
- **Source Server:** Public-facing upload server (accessible from Internet)
- **Access Control:** Provider-specific credentials (RUS2, NEO, GEO each have their own subdirectory)
- **Purpose:**
  - External providers upload content here
  - Workflow: `Incoming/` → `Processing/` → `Ready/` → `Archive/`
  - Playlists reference files from `PROVIDERS/[Provider]/Ready/`
- **Security:** VPN or secure transfer protocols, isolated per provider

#### ARCHIVE Directory
- **Mount Type:** Remote filesystem (NFS/SMBFS)
- **Source Server:** Dedicated archive/backup server
- **Access Control:** Internal only, automated archival processes
- **Purpose:**
  - Long-term storage of completed/old content
  - Backup and disaster recovery
  - Historical content retention
- **Retention:** Organized by year (2023/, 2024/, 2025/)

#### Local Directories (App Server)
All other directories (`CHANNELS/`, `COMMON/`, `LIBRARY/`, `WORK/`) are stored locally on the App Server (`192.168.238.162`) for fast access and playout operations.

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
- **Playlists use files from `PROVIDERS/[Provider]/Ready/`**
- Clear workflow: **Incoming → Processing → Ready → Archive**
- Easy to add new providers
- Track delivery status
- Provider name included in directory structure

### ✅ Flexibility
- `CUSTOM/` directories for future providers
- `GEO/` provider for Georgian content (magti-my specific)
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
| `SD/GEO/` | `PROVIDERS/GEO/Ready/SD/Movies/` (magti-my) |
| `SD/ENG/` | `PROVIDERS/[Provider]/Ready/SD/Movies/` |
| `HD/ADS2/` | `COMMON/Ads/HD/Active/` |
| `SD/ADs/` | `COMMON/Ads/SD/Active/` |
| `HD/HD BUMPERS/` | `COMMON/Bumpers/[channel]/HD/` |
| `SD/GEO BUMP/` | `COMMON/Bumpers/magti-my/SD/` |
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
- Update playout system paths to reference `PROVIDERS/[Provider]/Ready/`
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

### 6. Playlist Generation
- **Playlists reference files from `PROVIDERS/[Provider]/Ready/`**
- File codes must be included in all media files
- Provider name embedded in directory path

---

## Recommended File Naming

**All files MUST include file codes for database tracking.**

### Movies
**Format:** `[Title]_[Year]_[Language]_[Provider]_[FileCode].[ext]`

**Examples:**
- `Godzilla_2014_RUS_RUS2_1049A.mov` (RUS2 provider)
- `Georgian_Movie_2024_GEO_GEO_2890A.mov` (GEO provider for magti-my)

### Series
**Format:** `[Title]_S[NN]E[NN]_[Language]_[Provider]_[FileCode].[ext]`

**Examples:**
- `Friends_S01E01_ENG_NEO_2345A.mov`
- `Breaking_Bad_S02E05_RUS_RUS2_1087A.mov`

### Ads
**Format:** `[Brand]_[Campaign]_[Duration]_[Language]_[ValidUntil].[ext]`

**Example:** `Brand_Campaign_30sec_GEO_2025-12-31.mov`

### Bumpers
**Format:** `[Channel]_[Type]_[Duration]_[Language].[ext]`

**Examples:**
- `magti-hit_Ident_5sec_GEO.mov`
- `magti-my_Promo_10sec_GEO.mov`
- `magti-kino_Ident_3sec_GEO.mov`

---

**Last Updated:** 2025-12-11
