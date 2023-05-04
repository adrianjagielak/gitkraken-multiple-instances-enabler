A script for modifying GitKraken app to allow launching GitKraken in multiple windows/instances and removing the minimum width restriction of GitKraken window.

**Tested on macOS for GitKraken 8.2.1**

Copy & paste following script into Terminal and press enter:

```bash
echo "Closing GitKraken..."
killall GitKraken

echo "Creating a backup of app.asar..."
cp /Applications/GitKraken.app/Contents/Resources/app.asar /Applications/GitKraken.app/Contents/Resources/app-backup.asar

echo "Installing dependencies..."
brew install node

echo "Unpacking app.asar..."
npx --yes electron/asar extract /Applications/GitKraken.app/Contents/Resources/app.asar /Applications/GitKraken.app/Contents/Resources/app-extracted

echo "Removing single instance check in main.bundle.js..."
sed -i '' 's/ne.requestSingleInstanceLock()||process.exit(0),//g' /Applications/GitKraken.app/Contents/Resources/app-extracted/src/main/static/main.bundle.js

echo "Removing window width restriction..."
sed -i '' 's/minWidth:1024,/minWidth:100,/g' /Applications/GitKraken.app/Contents/Resources/app-extracted/src/main/static/main.bundle.js

echo "Re-packing app.asar..."
npx --yes electron/asar pack /Applications/GitKraken.app/Contents/Resources/app-extracted /Applications/GitKraken.app/Contents/Resources/app-patched.asar

echo "Replacing app.asar with patched app-patched.asar..."
mv /Applications/GitKraken.app/Contents/Resources/app-patched.asar /Applications/GitKraken.app/Contents/Resources/app.asar

echo "Cleanup (deleting extracted copy of app)..."
rm -rf /Applications/GitKraken.app/Contents/Resources/app-extracted

echo "Opening two instances of GitKraken..."
open -n /Applications/GitKraken.app
open -n /Applications/GitKraken.app

echo "Done"
```

You can open additional instances of GitKraken using `open -n /Applications/GitKraken.app` Terminal command or by duplicating GitKraken.app (e.g. "GitKraken 1", "GitKraken 2" etc)
