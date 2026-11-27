# Biblio 3.1127

## 🚀 Improvements
- Added a read-only metadata analysis report with an interactive view of extraction candidates, parsing outcomes, rule checks, and selection decisions.
- Improved extraction of year and month values from dates, including support for Italian textual date formats.
- Added configurable sums, averages, minimums, and maximums for numeric or amount metadata in saved document searches, with selected results shown on Home tiles.
- Added saved search management in Preferences, including renaming, deletion, and Home visibility.

## 🐞 Fixes
- Prevented locale-specific thousands separators from being added to numeric metadata values during editing.

## 🐳 Docker Images

| Image | Tag | Images |
|-------|------|------|
| domlan/biblio | 3.1127 | [Docker Hub](https://hub.docker.com/r/domlan/biblio/tags?name=3.1127)
| domlan/biblio-ocr | 3.1127 | [Docker Hub](https://hub.docker.com/r/domlan/biblio-ocr/tags?name=3.1127)
| domlan/deeplearning | 3.1127 | [Docker Hub](https://hub.docker.com/r/domlan/biblio-deeplearning/tags?name=3.1127)
| postgres | 14.5-alpine | [Docker Hub](https://hub.docker.com/layers/library/postgres/14.5-alpine)
| redis | 8.6.1-alpine | [Docker Hub](https://hub.docker.com/layers/library/redis/8.6.1-alpine)
| rabbitmq | 3.13-management-alpine | [Docker Hub](https://hub.docker.com/layers/library/rabbitmq/3.13-management-alpine)
| mongo | 8.0.3-noble | [Docker Hub](https://hub.docker.com/layers/library/mongo/8.0.3-noble)


## ⚠️ Notes
This release is backward compatible.
The PostgreSQL migration for saved search metadata is applied on application startup.
Conversion and alignment for the move to GridFS expected on first run. We recommend starting Docker Composition in attached mode (-d) and waiting for completion, as indicated in the console log.
