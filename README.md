PS C:\Windows\system32> $server = "CPEVBUAPPP16"
PS C:\Windows\system32> Invoke-Command -ComputerName $server -ScriptBlock {
>>     $svc = "CSFalconService"
>>     $service = Get-Service -Name $svc -ErrorAction SilentlyContinue
>>     if ($null -eq $service) {
>>         Write-Output "CrowdStrike Falcon service is NOT installed."
>>     } elseif ($service.Status -eq "Running") {
>>         Write-Output "CrowdStrike Falcon service is RUNNING."
>>     } else {
>>         Write-Output "CrowdStrike Falcon service is $($service.Status). Attempting to start..."
>>         try {
>>             Start-Service -Name $svc
>>             Start-Sleep -Seconds 3
>>             $service = Get-Service -Name $svc
>>             if ($service.Status -eq "Running") {
>>                 Write-Output "Service started successfully."
>>             } else {
>>                 Write-Output "Service failed to start. Status: $($service.Status)"
>>             }
>>         } catch {
>>             Write-Output "ERROR: $_"
>>         }
>>     }
>> }
