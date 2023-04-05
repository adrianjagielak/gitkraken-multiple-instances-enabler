A script for modifying GitKraken app to allow launching GitKraken in multiple windows/instances.

**Tested on macOS for GitKraken 8.2.1**

Copy & paste following script into Terminal and press enter:

```bash
echo "Close GitKraken"
killall GitKraken

echo "Make a backup of app.asar"
cp /Applications/GitKraken.app/Contents/Resources/app.asar /Applications/GitKraken.app/Contents/Resources/app-backup.asar

echo "Install dependencies"
brew install node

echo "Unpack app.asar"
npx --yes electron/asar extract /Applications/GitKraken.app/Contents/Resources/app.asar /Applications/GitKraken.app/Contents/Resources/app-extracted

echo "Remove single instance check in main.bundle.js"
sed -i '' 's/ne.requestSingleInstanceLock()||process.exit(0),//g' /Applications/GitKraken.app/Contents/Resources/app-extracted/src/main/static/main.bundle.js

echo "Remove GitKraken window width restriction"
sed -i '' 's/minWidth:1024,/minWidth:100,/g' /Applications/GitKraken.app/Contents/Resources/app-extracted/src/main/static/main.bundle.js

echo "Re-pack app.asar"
npx --yes electron/asar pack /Applications/GitKraken.app/Contents/Resources/app-extracted /Applications/GitKraken.app/Contents/Resources/app-patched.asar

echo "Replace app.asar with patched app-patched.asar"
mv /Applications/GitKraken.app/Contents/Resources/app-patched.asar /Applications/GitKraken.app/Contents/Resources/app.asar

echo "Cleanup (delete extracted copy of app)"
rm -rf /Applications/GitKraken.app/Contents/Resources/app-extracted

echo "Open two instances of GitKraken"
open -n /Applications/GitKraken.app
open -n /Applications/GitKraken.app
```

You can open additional instances of GitKraken using `open -n /Applications/GitKraken.app` Terminal command.
