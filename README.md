# Blackfire for Nix

This repository contains the Nix packages for the Blackfire agent and the Blackfire PHP extension.

## Usage in Devenv

### devenv.yaml

```diff
inputs:
  nixpkgs:
    url: github:NixOS/nixpkgs/nixpkgs-unstable
+  blackfire:
+    url: github:shyim/blackfire-nix
+    inputs:
+      nixpkgs:
+        follows: nixpkgs
```

### devenv.nix

```diff
-{ pkgs, lib, config, ... }:
+{ pkgs, lib, config, inputs, ... }:

{
+  services.blackfire.package = inputs.blackfire.packages.${builtins.currentSystem}.blackfire;

  languages.php.package = lib.mkDefault (pkgs.php.buildEnv {
-    extensions = { all, enabled }: with all; enabled ++ [ amqp redis grpc blackfire ];
+    extensions = { all, enabled }: with all; enabled ++ [ amqp redis grpc inputs.blackfire.packages.${builtins.currentSystem}.blackfire-php ];
}
```
