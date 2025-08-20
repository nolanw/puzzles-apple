# Puzzles for Apple platforms

[Simon Tatham](https://www.chiark.greenend.org.uk/~sgtatham/) has a [Portable Puzzle Collection](https://www.chiark.greenend.org.uk/~sgtatham/puzzles/). Unfortunately they no longer provide updated official macOS builds. And there's never been official builds for iPadOS, iOS, or other current Apple platforms. We can help!

## Development goals

1. Contribute upstream whenever possible.
2. Keep our use of the upstream repo clean and tidy, so we can easily pull updates.

## Build the Mac app

Make sure you've cloned submodules:

```sh
git submodule update --init --recursive
```

You need CMake. See top of `puzzles/CMakeLists.txt` for version requirements. I install CMake using MacPorts.

Set the necessary environment variables (I use [`direnv`](https://direnv.net) for convenience):

* `MAC_BUNDLE_IDENTIFIER`: The bundle identifer you'd like to use for the built Mac app.
* `MAC_CODESIGN_CERT_NAME`: The name of a Developer ID certificate in your keychain, to sign the built Mac app.
* `NOTARY_APPLE_ID`: An Apple ID with an active Apple Developer membership, for notarizing the Mac app.
* `NOTARY_KEYCHAIN_PROFILE`: The profile name you saved your app-specific password under using `xcrun notarytool store-credentials`, for notarizing the Mac app.
* `NOTARY_TEAM_ID`: The Apple Developer team ID to use for notarizing the Mac app.

Then run the build script:
```sh
script/build
```

You should find a `scratch/Puzzles.zip` ready for distribution.

### Automated builds

A GitHub Actions workflow [`release-mac-app`](.github/workflows/release-mac-app.yml) automatically does the above and adds the result to a new Release. The following secrets should be set for your GitHub repository:

* `CI_KEYCHAIN_PASSWORD`: A password used to protect the newly-created keychain on each CI run. Can be whatever you like, keychains just need to have a password.
* `MAC_CODESIGN_CERT_BASE64`: A Developer ID certificate plus private key, exported from Keychain Access (using a password, specified separately) and base64-encoded. To export:
    1. Locate the certificate and its private key in Keychain Access.
    2. Select both the certificate and its private key, then right-click and choose "Export 2 items…".
    3. Choose "Personal Information Exchange (.p12)" format.
    4. Save the file somewhere.
    5. Input a generated password from somewhere (and remember it!).
    6. Base64-encode the file and copy it:
    ```sh
    base64 --input CoolCert.p12 | pbcopy
    ```
    7. Paste the base64-encoded certificate and private key into your GitHub repo's secrets.
* `MAC_CODESIGN_CERT_NAME`: The full name of your Develpoer ID certificate, as seen in Keychain Access.
* `MAC_CODESIGN_CERT_PASSWORD`: The password you chose when exporting your Developer ID certificate and private key.
* `NOTARY_APP_SPECIFIC_PASSWORD`: An app-specific password from your Apple Developer account, used to notarize the built app.
* `NOTARY_APPLE_ID`: The Apple ID for your Apple Developer account that you want to use for notarization.
* `NOTARY_TEAM_ID`: Your Apple Developer team ID that you want to use for notarization. To find your team ID:
    1. Log in to your Apple Developer account.
    2. Go to the Membership page.
    3. Look for a "Team ID" field.

Plus one variable:

* `MAC_BUNDLE_IDENTIFIER`: The ID you'd like to use for the built app bundle.

