Para cambiar el prompt de la consola:

**en power shell ejecutar:**

```
New-Item -Path $PROFILE -ItemType File -Force
```

esto crea un archivo en 

```
C:\Users\tu_usuario\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```
una vez creado hacer algo asi como:

```
C:\Users\tu_usuario\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```
y entonces ya se puede crear:

```notepad $PROFILE```

con este codigo como ejemplo:

``` 
function prompt {
    # Carpeta actual (solo el nombre)
    $dir = Split-Path -Leaf (Get-Location)

    # Detectar venv activo (Python)
    $venv = ""
    if ($env:VIRTUAL_ENV) {
        $venv = " [" + (Split-Path -Leaf $env:VIRTUAL_ENV) + "]"
    }

    # Detectar git (rama + edi
    $git = ""
    try {
        # Detecta rama aunque estés en subcarpetas (no solo si existe .git aquí)
        $inside = git rev-parse --is-inside-work-tree 2>$null
        if ($inside -eq "true") {
            $branch = git branch --show-current 2>$null
            if (-not $branch) { $branch = "detached" }

            $dirty = ""
            $status = git status --porcelain 2>$null
            if ($status) { $dirty = "*" }

            $git = " ($branch$dirty)"
        }
    } catch {}

    "$dir$git > "
}
```

una vez salvado se puede recargar el perfil

```
.$PROFILE
```

o se puede reiniciar la terminal en VSCode