
name: Build NYIMBO YA KUTOTA LESA APK

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Extract Android project
        run: |
          mkdir -p source
          unzip -q NYIMBO_YA_KUTOTA_LESA_Android_Project.zip -d source
          echo "PROJECT_DIR=$(find source -mindepth 1 -maxdepth 1 -type d | head -n 1)" >> "$GITHUB_ENV"

      - name: Set up Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Set up Android SDK
        uses: android-actions/setup-android@v3

      - name: Build APK
        run: |
          cd "$PROJECT_DIR"
          gradle assembleDebug --no-daemon

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: NYIMBO_YA_KUTOTA_LESA-APK
          path: source/**/app/build/outputs/apk/debug/*.apk
          if-no-files-found: error
