# Puzzles for Apple platforms

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
