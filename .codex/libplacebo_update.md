# libplacebo 7.360.1 更新記録

## セッション情報

- Codex session ID: `019fceed-24b2-7c83-ac28-4068c31a6bb0`

## 更新理由

libplacebo関連フィルタで発生していたSPIR-V不整合を解消するため、libplaceboを7.360.1へ更新した。

詳細は [Oomugi413/Amatsukaze/.codex/CUDA13-PTXfix.md](https://github.com/Oomugi413/Amatsukaze/blob/master/.codex/CUDA13-PTXfix.md) を参照。

## 変更内容

対象ファイル: `ffmpeg_dll/build_ffmpeg_dll.sh`

- libplacebo: `v7.351.0` → `v7.360.1`
- shaderc: `v2024.1` → `v2026.2`
- Vulkan Loader/Header: `v1.3.295` → `v1.4.356`

libplacebo 7.360.1が要求するVulkan 1.4およびshadercの対応環境を揃え、Vulkan stub版ではなくVulkan実装版を生成できる構成にした。

## Vulkan Loader 1.4.356 対応パッチの修正

Vulkan Loader/Headerを1.4.356へ更新した後、GitHub Actionsのbase imageビルドで、staticリンク用パッチが旧版のソース構造を前提としていたため適用に失敗した。

対象ファイル `ffmpeg_dll/patches/vulkan_loader_static.diff` をVulkan Loader 1.4.356対応版へ更新した。主な変更は以下のとおり。

- `loader/CMakeLists.txt` のstaticライブラリ化およびリンク指定を1.4.356の構造に合わせて修正
- `loader/loader_windows.c` のstaticビルド向け修正を1.4.356のソース位置に合わせて更新
- `loader/vulkan.pc.in` のprivate library情報の反映を維持
- 旧版専用の `loader/vk_loader_platform.h` hunkを削除

実際のVulkan Loader v1.4.356ソースに対する `patch --dry-run` が成功することを確認した。
