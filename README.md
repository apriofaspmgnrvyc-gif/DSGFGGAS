name: Build Mod

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
      
      - name: Install Gradle
        run: |
          wget -q https://services.gradle.org/distributions/gradle-8.8-bin.zip
          unzip -q gradle-8.8-bin.zip
          export PATH=$PATH:$PWD/gradle-8.8/bin
          echo "$PWD/gradle-8.8/bin" >> $GITHUB_PATH
      
      - name: Build
        run: cd tntmod && gradle build
      
      - name: Upload mod
        uses: actions/upload-artifact@v4
        with:
          name: BVCartHelper-mod
          path: tntmod/build/libs/*.jar
