---
title: PowerShell を使用して Azure 内の Windows VM に データ ディスクを接続する | Microsoft Docs
description: Resource Manager デプロイ モデルで、PowerShell を使用して Windows VM に新規または既存のデータ ディスクを接続する方法です。
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: b293a60240622b5c8e417bf61de83ae8817e203f
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477126"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>PowerShell を使用して Windows VM にデータ ディスクを接続する

この記事では、PowerShell を使用して新しいディスクおよび既存のディスクを Windows 仮想マシンに接続する方法について説明します。 

最初に、以下のヒントを確認してください。
* 仮想マシンのサイズによって、接続できるデータ ディスク数は変わります。 詳細については、 [仮想マシンのサイズ](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)に関するページをご覧ください。
* Premium Storage を使用するには、Premium Storage に対応した VM の種類 (DS シリーズや GS シリーズなどの仮想マシン) が必要です。 詳細については、[Premium Storage:Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ (Premium Storage)](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) に関する記事を参照してください。

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell をローカルにインストールして使用する場合、このチュートリアルでは Azure PowerShell モジュール バージョン 6.0.0 以降が必要になります。 バージョンを確認するには、` Get-Module -ListAvailable AzureRM` を実行します。 アップグレードする必要がある場合は、[Azure PowerShell モジュールのインストール](/powershell/azure/azurerm/install-azurerm-ps)に関するページを参照してください。 PowerShell をローカルで実行している場合、`Connect-AzureRmAccount` を実行して Azure との接続を作成することも必要です。


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>空のデータ ディスクを仮想マシンに追加する

この例では、既存の仮想マシンに空のデータ ディスクを追加する方法を示します。

### <a name="using-managed-disks"></a>マネージド ディスクを使用する場合

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>可用性ゾーンでマネージド ディスクを使用する場合
可用性ゾーンにディスクを作成するには、`-Zone` パラメーターを指定して [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) を使用します。 次の例では、ゾーン *1* にディスクを作成します。


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>ディスクの初期化

空のディスクは、追加した後で初期化する必要があります。 ディスクを初期化するには、VM にサインインしてディスクの管理を使用します。 VM を作成したときに [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) と証明書を有効にした場合は、リモート PowerShell を使用してディスクを初期化できます。 また、カスタム スクリプト拡張機能を使用することができます。 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
スクリプト ファイルには、たとえば、ディスクを初期化するためのコードを含めることができます。

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>VM に既存のデータ ディスクを接続する

既存のマネージド ディスクをデータ ディスクとして VM にアタッチできます。 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>次の手順

[スナップショット](snapshot-copy-managed-disk.md)を作成します。
