---
title: Concatenar diferentes archivos de Excel en una sola hoja de Excel
author: Francisco Sola
date: 2020-06-06 11:06:00 -0
categories: [Blogging, Excel]
tags: [Visual Basic, code, Macros]
---

### Juntar varias hojas de Excel de archivos diferentes en una única hoja


```visualbasic
    
    Sub juntarExcel()
		Dim bookList As Workbook
		Dim objeto As Object, directorio As Object, archivos As Object, archivo As Object
		Application.ScreenUpdating = True

		Set objeto = CreateObject("Scripting.FileSystemObject")
		Set directorio = objeto.Getfolder(PATH)
		Set archivos = directorio.Files

		For Each archivo In archivos
		    Set bookList = Workbooks.Open(archivo)
		    
		    Debug.Print bookList.Name
		    
		    bookList.Worksheets(1).Activate
		        
		    Lastrow = ActiveSheet.Cells(Rows.Count, 1).End(xlUp).Row
		    Lastcolumn = ActiveSheet.Cells(5, Columns.Count).End(xlToLeft).Column
		    
		    Debug.Print Lastrow
		    Debug.Print Lastcolumn
		    
		    Range(Cells(6, 1), Cells(Lastrow, Lastcolumn)).Copy
		   
		   
		    ThisWorkbook.Worksheets(1).Activate
		    
		    endrow = ActiveSheet.Cells(Rows.Count, 1).End(xlUp).Offset(1, 0).Row

		    ActiveSheet.Paste Destination:=ActiveSheet.Range(Cells(endrow, 1), Cells(endrow, 5))
		    
		    
		    Application.CutCopyMode = False
		    bookList.Close
		Next

	End Sub

```
