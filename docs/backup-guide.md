# File Backup Guide / ファイルバックアップガイド

This guide explains how to upload files from your device's download folder (`~/storage/downloads`) to another device for backup purposes.

このガイドでは、デバイスのダウンロードフォルダ（`~/storage/downloads`）から別のデバイスにファイルをアップロードしてバックアップする方法を説明します。

---

## 📱 Overview / 概要

The path `~/storage/downloads` is commonly found on Android devices (especially when using terminal apps like Termux). Below are several methods to transfer files from this folder to another device.

`~/storage/downloads` パスは、Androidデバイス（特にTermuxなどのターミナルアプリを使用する場合）でよく見られます。以下に、このフォルダから別のデバイスにファイルを転送する方法をいくつか紹介します。

---

## 🔄 Transfer Methods / 転送方法

### Method 1: Cloud Storage / クラウドストレージ

**Recommended for: Most users / おすすめ: ほとんどのユーザー**

1. **Google Drive**
   - Open the Files app or Google Drive app on your device
   - Navigate to your Downloads folder
   - Select the files you want to backup
   - Tap "Share" → "Save to Drive"
   - Access files on another device by logging into Google Drive
   
   手順：Filesアプリまたは Google Drive アプリを開く → ダウンロードフォルダへ移動 → ファイルを選択 → 「共有」→「ドライブに保存」

2. **Dropbox / OneDrive / iCloud**
   - Similar process: install the app, select files, and upload
   - Access from any device with the same account
   
   同様の手順で、アプリをインストールしてファイルをアップロードできます

---

### Method 2: USB Cable Transfer / USBケーブル転送

**Recommended for: Large files / おすすめ: 大きなファイル**

1. Connect your device to a computer using a USB cable
2. On your device, select "File Transfer" or "MTP" mode when prompted
3. On your computer, open the device storage
4. Navigate to `Download` or `storage/downloads`
5. Copy files to your computer or another device

手順：
1. USBケーブルでデバイスをコンピュータに接続
2. デバイスで「ファイル転送」または「MTP」モードを選択
3. コンピュータでデバイスストレージを開く
4. `Download` または `storage/downloads` に移動
5. ファイルをコンピュータまたは別のデバイスにコピー

---

### Method 3: Using Termux (Android) / Termuxを使用（Android）

**Recommended for: Technical users / おすすめ: 技術的なユーザー**

If you're using Termux on Android:

```bash
# Grant storage permission first (a permission dialog will appear on your device)
termux-setup-storage

# Navigate to downloads folder
cd ~/storage/downloads

# List files
ls -la

# Copy files using various methods:

# Option A: Use scp (requires ssh installed and running on target device)
scp filename.txt user@target-device-ip:/path/to/destination/

# Option B: Use rsync (recommended for large transfers, requires ssh on target)
rsync -avz --progress ~/storage/downloads/ user@target-device-ip:/backup/

# Option C: Create a compressed archive
tar -czvf backup.tar.gz ~/storage/downloads/
# Then transfer the archive file
```

> ⚠️ **Security Note / セキュリティに関する注意:**
> - Use SSH key authentication instead of passwords for scp/rsync transfers
> - Ensure you're on a trusted network when transferring files
> - Consider using `ssh-keygen` to create key pairs for secure authentication
> 
> scp/rsync転送にはパスワードの代わりにSSHキー認証を使用してください。ファイル転送時は信頼できるネットワーク上にいることを確認してください。

---

### Method 4: Local Network Transfer / ローカルネットワーク転送

**Recommended for: Quick transfers between nearby devices / おすすめ: 近くのデバイス間の素早い転送**

1. **Using HTTP Server (Termux)**
   ```bash
   cd ~/storage/downloads
   python -m http.server 8080
   ```
   Then access `http://your-device-ip:8080` from another device on the same network.

   > ⚠️ **Warning / 警告:** This HTTP server has no authentication. Only use on trusted private networks and stop the server immediately after transfer. 
   > このHTTPサーバーには認証がありません。信頼できるプライベートネットワークでのみ使用し、転送後すぐにサーバーを停止してください。

2. **Using Apps**
   - **Send Anywhere**: Cross-platform file sharing
   - **Snapdrop**: Browser-based, works on the same network
   - **AirDroid**: Wireless file management

---

### Method 5: Bluetooth Transfer / Bluetooth転送

**Recommended for: Small files / おすすめ: 小さなファイル**

1. Enable Bluetooth on both devices
2. Pair the devices
3. On your device, open Files app → Downloads folder
4. Select files → Share → Bluetooth
5. Choose the target device

手順：両方のデバイスでBluetoothを有効化 → ペアリング → ファイルを選択 → 共有 → Bluetooth

---

## 📋 Quick Reference / クイックリファレンス

| Method | Best For | Speed | Ease |
|--------|----------|-------|------|
| Cloud Storage | Remote backup | Medium | ⭐⭐⭐⭐⭐ |
| USB Cable | Large files | Fast | ⭐⭐⭐⭐ |
| Termux (scp/rsync) | Automation | Fast | ⭐⭐ |
| Local Network | Quick sharing | Fast | ⭐⭐⭐ |
| Bluetooth | Small files | Slow | ⭐⭐⭐ |

---

## 🔧 Troubleshooting / トラブルシューティング

### Cannot access ~/storage/downloads in Termux
```bash
# Run this command to grant storage access
termux-setup-storage
# Then restart Termux
```

### Files not showing up
- Ensure the storage permission is granted in your device settings
- Try refreshing the file manager
- Check if files are hidden (start with `.`)

### Transfer fails
- Check available space on the target device
- Ensure stable network connection for network transfers
- Try smaller batch sizes for large number of files

---

## 📚 Additional Resources / 追加リソース

- [Termux Wiki - Storage](https://wiki.termux.com/wiki/Internal_and_external_storage)
- [Android File Transfer Guide](https://support.google.com/android/answer/9064445)
- [Google Drive Help](https://support.google.com/drive)

---

> **Tip / ヒント:** For regular backups, consider setting up an automated script using Termux and cron jobs, or use cloud services with auto-sync enabled.
> 
> 定期的なバックアップには、Termuxとcronジョブを使用した自動スクリプトの設定、または自動同期が有効なクラウドサービスの使用を検討してください。
