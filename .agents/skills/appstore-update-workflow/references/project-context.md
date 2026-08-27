# Project Context

Use this reference to orient repository work. Verify it against the current code before relying on details that may have changed.

## Repository Shape

- The Git root contains business/test workbooks such as `demo.xlsx`, `会员.xlsx`, and the `金币*.xlsx` files.
- The root `index.html` is an offline, double-clickable operation console that selects local workbook names and generates complete Codex instructions. It intentionally does not read credentials, parse workbook contents, or perform live uploads.
- `上传操作说明.md` is the Chinese operator guide for workbook preparation, AppleParty uploads, operations-backend imports, and no-Xcode usage options.
- The macOS application lives in `AppleParty/` and is built from `AppleParty/AppleParty.xcworkspace` with the `AppleParty` scheme.
- The app is primarily AppKit/Swift. CocoaPods supplies Alamofire, CoreXLSX, Sparkle, and other UI/utilities; Swift Package Manager supplies `appstoreconnect-swift-sdk` and related packages.
- Open the `.xcworkspace`, not the `.xcodeproj`, so CocoaPods dependencies are available.

## Excel-To-App-Store Flow

1. `APInAppPurchseVC.handelExcel` receives a workbook and calls `IAPExcelParser.parser`.
2. The parser reads four logical sheets into `IAPProduct` and its nested models:
   - `AppleParty`: product/group identity, product type, subscription period, review data, and English localization fields.
   - `PricePoints`: recurring/base price and optional territory-specific manual prices.
   - `IntroductoryOffers`: base-territory introductory offer. The current documented first-week offer is `PAY_AS_YOU_GO`, `ONE_WEEK`, one period; a blank start date resolves on upload day and a blank end date means no fixed end.
   - `Territories`: all-territory flags or an explicit territory list.
3. `APInAppPurchseVC` warns about duplicate product IDs/reference names, retains entries that need metadata, and keeps auto-renewable subscriptions eligible when an introductory offer still needs processing.
4. `APUploadIAPListVC` previews the parsed products and requires an App Store Connect API key before upload.
5. Non-renewing IAPs and auto-renewable subscriptions use separate create/update flows. Each flow handles relevant localization, screenshots, availability, and prices. Subscription flow also handles introductory offers and optional review submission.
6. `AppStoreConnectAPI.swift` contains the App Store Connect request helpers. Upload logs are streamed into `APDebugVC`; failures are accumulated by product and summarized at the end.

## Important Workbook Contracts

- Sheet names and column positions are parser contracts, not presentation-only labels.
- Product IDs join rows across `AppleParty`, `PricePoints`, `IntroductoryOffers`, and `Territories`.
- The parser supports shared strings and inline strings because Excel, Numbers, WPS, and generated OOXML may encode cells differently.
- Prices are normalized before matching App Store Connect price points.
- `IntroductoryOffers` permits at most one base offer row per product. For some existing product-ID conventions, the parser can infer a first-week price only when the encoded recurring price matches `PricePoints`.
- Root workbooks and `AppleParty/AppleParty/Resources/InAppPurchase/example.xlsx` are behavior-sensitive fixtures. Preserve unrelated workbook content when editing them.

## Primary Files

- Excel parsing and input models: `AppleParty/AppleParty/AppListView/InAppPurchseView/Models/IAPExcelParser.swift`
- Workbook intake and preview routing: `AppleParty/AppleParty/AppListView/InAppPurchseView/APInAppPurchseVC.swift`
- Upload orchestration and failure logs: `AppleParty/AppleParty/AppListView/InAppPurchseView/APUploadIAPListVC.swift`
- App Store Connect API helpers: `AppleParty/AppleParty/Shared/Network/AppStoreConnectAPI.swift`
- Parser coverage: `AppleParty/ApplePartyTests/ApplePartyTests.swift`
- User-facing workflow notes: `AppleParty/README.md`
- Offline operator console: `index.html`
- Chinese upload guide: `上传操作说明.md`

## Verification

List schemes:

```sh
xcodebuild -list -workspace AppleParty/AppleParty.xcworkspace
```

Run macOS tests:

```sh
xcodebuild test \
  -workspace AppleParty/AppleParty.xcworkspace \
  -scheme AppleParty \
  -destination 'platform=macOS'
```

For parser or workbook changes, add or update focused XCTest cases and verify the relevant workbook can be opened by CoreXLSX. Live App Store Connect calls are not part of routine verification.
