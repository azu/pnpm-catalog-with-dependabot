`pnpm install --frozen-lockfile` does not throw an error when the lockfile is out of sync with pnpm-workspace.yaml's catalog.

## pnpm-workspace.yaml

- refer `lodash@4.0.0` in the workspace file

```json5
catalog:
  lodash: 4.0.0
```

## pnpm-lock.yaml

- refer `lodash@4.17.21` in the lockfile

```yaml
lockfileVersion: '9.0'

settings:
  autoInstallPeers: true
  excludeLinksFromLockfile: false

catalogs:
  default:
    lodash:
      specifier: 4.17.21
      version: 4.17.21

importers:

  .:
    dependencies:
      lodash:
        specifier: 'catalog:'
        version: 4.17.21

packages:

  lodash@4.17.21:
    resolution: {integrity: sha512-v2kDEe57lecTulaDIuNTPy3Ry4gLGJ6Z1O3vE1krgXZNrsQ+LFTGHVxVjcXPs17LhbZVGedAJv8XZ1tvj5FvSg==}

snapshots:

  lodash@4.17.21: {}

```


## Reproduce Steps

1. Run `pnpm install --frozen-lockfile` in the workspace root directory

```
pnpm install --frozen-lockfile
```

However, the command does not throw an error in `pnpm@10.7.1`.

## Expected Behavior

- The command should throw an error indicating that the lockfile is out of sync with the pnpm-workspace.yaml.

## Actual Behavior

- The command does not throw an error, and the lockfile is not updated to match the package.json.

Also, the command `pnpm install` updates the lockfile to match the pnpm-workspace.yaml.

```bash
pnpm install
git diff
diff --git i/pnpm-lock.yaml w/pnpm-lock.yaml
index 41fa5a5..7d83bb3 100644
--- i/pnpm-lock.yaml
+++ w/pnpm-lock.yaml
@@ -7,8 +7,8 @@ settings:
 catalogs:
   default:
     lodash:
-      specifier: 4.17.21
-      version: 4.17.21
+      specifier: 4.0.0
+      version: 4.0.0

 importers:

@@ -16,13 +16,13 @@ importers:
     dependencies:
       lodash:
         specifier: 'catalog:'
-        version: 4.17.21
+        version: 4.0.0

 packages:

-  lodash@4.17.21:
-    resolution: {integrity: sha512-v2kDEe57lecTulaDIuNTPy3Ry4gLGJ6Z1O3vE1krgXZNrsQ+LFTGHVxVjcXPs17LhbZVGedAJv8XZ1tvj5FvSg==}
+  lodash@4.0.0:
+    resolution: {integrity: sha512-bWpSlBobTcHYK9eUzcBYHhSBGzvSzEsxocnW5+v7p6wCRlY1icneTe2ACam3mGdAu82+RLL32cmyl7TRlJHqZw==}

 snapshots:

-  lodash@4.17.21: {}
+  lodash@4.0.0: {}
```

