![Microsoft Cloud Workshop](images/ms-cloud-workshop.png)

Deploy Azure Resources by using GitHub Actions  
Hands-on lab  
Jan 2021

<br />

### リソース作成時に指定する項目のパラメーター化
※ Parameters セクションに記述
```
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "ストレージ アカウント名"
            }
        },
        "storageSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
                "Standard_LRS",
                "Standard_GRS",
                "Standard_ZRS",
                "Premium_LRS",
                "Premium_ZRS"
            ],
            "metadata": {
                "description": "レプリケーションの種類"
            }
        },
        "resourceTag": {
            "type": "object",
            "defaultValue": {
                "Department": "Dev",
                "Environment": "Productoin"
            },
            "metadata": {
                "description": "タグ（名前：値）"
            }
        }
```

<br />

※ variables セクションに記述
```
        "storageTier": "[if(contains(parameters('storageSku'), 'Standard'), 'Standard', 'Premium')]"
```

<br />

※ resources セクションを上書き
```
            "name": "[parameters('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2019-06-01",
            "tags": "[parameters('resourceTag')]",
            "location": "[resourceGroup().location]",
            "kind": "StorageV2",
            "sku": {
                "name": "[parameters('storageSku')]",
                "tier": "[variables('storageTier')]"
            }
```