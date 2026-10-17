name: Build APK
on:
  push:
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - run: npm install
      - run: npm install --no-save tesseract.js@5.1.1 @tesseract.js-data/eng@1.0.0
      - run: |
          test -f index.html || (echo 'index.html repo-la illa' && exit 1)
          test -f app.html || (echo 'app.html repo-la illa - upload pannunga' && exit 1)
          mkdir -p www/ocr
          cp index.html www/index.html
          cp app.html www/app.html
          cp node_modules/xlsx/dist/xlsx.full.min.js www/
          cp node_modules/tesseract.js/dist/tesseract.min.js node_modules/tesseract.js/dist/worker.min.js www/ocr/
          cp node_modules/tesseract.js-core/tesseract-core-lstm.wasm.js node_modules/tesseract.js-core/tesseract-core-simd-lstm.wasm.js www/ocr/
          cp node_modules/@tesseract.js-data/eng/4.0.0_best_int/eng.traineddata.gz www/ocr/
      - run: npx cap add android
      - run: npx cap sync android
      - run: |
          chmod +x android/gradlew
          cd android && ./gradlew assembleDebug
      - uses: actions/upload-artifact@v4
        with:
          name: duty-deployment-apk
          path: android/app/build/outputs/apk/debug/app-debug.apk
