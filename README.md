# SLpGetLicenseAcquisitionInfo 函数
```c
  <DllImport("sppc.dll", EntryPoint:="SLpGetLicenseAcquisitionInfo", ExactSpelling:=False, CharSet:=CharSet.Unicode)>
    Public Function SLpGetLicenseAcquisitionInfo(ByVal hSLC As IntPtr, ByVal pwszPKeyAlgorithm As IntPtr, ByVal uBytes As UInteger, ByVal pData As IntPtr, ByRef peDataType As SLDATATYPE, ByRef pcbValue As UInteger, ByRef ppbValue As IntPtr) As Integer
    End Function

```
```c
                Dim pData As IntPtr = Marshal.AllocHGlobal(&H40)
                Dim bytes1() As Byte = {&H55, &H45, &HED, &HEA, &H1, &H0, &H0, &H0, &H22, &H0, &H0, &H0, &H4, &H0, &H0, &H0}
                Dim bytes2() As Byte = {&H0, &H0, &H0, &H0, &H0, &H0, &H0, &H0, &H1, &H0, &H0, &H0, &H0, &H0, &H0, &H0}
                Dim bData = ConcatByteArrays(bytes1, Encoding.Unicode.GetBytes("SppLAPPingServer"), bytes2)
                Marshal.Copy(bData, 0, pData, bData.Length)
                Dim hRes = SLpGetLicenseAcquisitionInfo(hSLC, Marshal.StringToHGlobalUni("msft:spp/licenseacquisition/payloadhandler/pa/1.0"), &H40, pData, New SLDATATYPE, pcbValue, ppbValue)
                If hRes = 0 Then
                    Dim bAlgorithm(pcbValue - 1) As Byte
                    Marshal.Copy(ppbValue, bAlgorithm, 0, pcbValue)
                    Debug.Print(BitConverter.ToString(bAlgorithm))
                    Debug.Print(Encoding.UTF8.GetString(bAlgorithm, 0, bAlgorithm.Length))
                End If
                Marshal.FreeHGlobal(pData)
                Dim byte1() As Byte = New Byte() {&H6C, &HF1, &HA, &H13, &H1, &H0, &H0, &H0, &H2A, &H0, &H0, &H0, &H4, &H0, &H0, &H0}
                Dim byte2() As Byte = New Byte() {&H83, &H42, &H87, &H86, &H2, &H0, &H0, &H0}
                Dim byte3() As Byte = New Byte() {&H18, &H0, &H0, &H0, &H4A, &H0, &H0, &H0}
                Dim byte4() As Byte = New Byte() {&H55, &HD6, &HAB, &H7C, &H1, &H0, &H0, &H0, &H1A, &H0, &H0, &H0, &H4, &H0, &H0, &H0}
                Dim byte5() As Byte = New Byte() {0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0}
                bData = ConcatByteArrays(byte1, Encoding.Unicode.GetBytes("SppLAPActivationType"), Encoding.ASCII.GetBytes(New String(ChrW(0), 16)), byte2, byte3, Encoding.Unicode.GetBytes("SppLAPSkuId" + ChrW(0) + pSkuId.ToString), Encoding.ASCII.GetBytes(New String(ChrW(0), 8)), byte4, Encoding.Unicode.GetBytes("SppRequirePL"), byte5)
                Debug.Print(String.Join(String.Empty, Array.ConvertAll(bData, Function(x) x.ToString("X2") & " ")))
                pData = Marshal.AllocHGlobal(&HF8)
                Marshal.Copy(bData, 0, pData, bData.Length)
                hRes = SLpGetLicenseAcquisitionInfo(hSLC, Marshal.StringToHGlobalUni("msft:spp/licenseacquisition/sequence/1.0"), &HF8, pData, New SLDATATYPE, pcbValue, ppbValue)
                If hRes = 0 Then
                    If pcbValue = &H290 Then
                        Dim bAlgorithm(pcbValue - 1) As Byte
                        Marshal.Copy(ppbValue, bAlgorithm, 0, pcbValue)
                        Dim enc = Encoding.Unicode.GetString(bAlgorithm, &H10, &H62)
                    End If
                End If
                
                hRes = SLpGetLicenseAcquisitionInfo(hSLC, Marshal.StringToHGlobalUni("msft:spp/licenseacquisition/payloadhandler/pa/1.0"), &HF8, pData, New SLDATATYPE, pcbValue, ppbValue)
                If hRes = 0 Then
                    Dim bAlgorithm(pcbValue) As Byte
                    Marshal.Copy(ppbValue, bAlgorithm, 0, pcbValue)
                    Dim index = SearchBytePattern(Encoding.Unicode.GetBytes("PublishLicense"), bAlgorithm)
                End If
                Marshal.FreeHGlobal(pData)
```

# HookSLpGetLicenseAcquisitionInfo

拦截SLpGetLicenseAcquisitionInfo函数获取License各种信息。


![image](https://github.com/laomms/HookSLpGetLicenseAcquisitionInfo/blob/master/111.png)

