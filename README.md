Sub loopticker()

    For Each ws In Worksheets

        Dim ticker As String
        Dim totalvolume As Double
        Dim ticker_row As Integer
        Dim open_price As Double
        Dim close_price As Double
        Dim yearly_change As Double
        Dim percent_change As Double
        
        ticker_row = 2
        open_price = ws.Cells(2, 3).Value
        totalvolume = 0
        
        ws.Cells(1, 9).Value = "Ticker"
        ws.Cells(1, 10).Value = "Yearly Change"
        ws.Cells(1, 11).Value = "Percent Change"
        ws.Cells(1, 12).Value = "Total Stock Volume"

        lastrow = ws.Cells(Rows.Count, 1).End(xlUp).Row


        For i = 2 To lastrow

            If ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
        
              ticker = ws.Cells(i, 1).Value

              totalvolume = totalvolume + ws.Cells(i, 7).Value

              ws.Range("I" & ticker_row).Value = ticker

              ws.Range("L" & ticker_row).Value = totalvolume
              
              close_price = ws.Cells(i, 6).Value

               yearly_change = (close_price - open_price)
              
              ws.Range("J" & ticker_row).Value = yearly_change

                If open_price = 0 Then
                    percent_change = 0
                
                Else
                    percent_change = yearly_change / open_price
                
                End If

              ws.Range("K" & ticker_row).Value = percent_change
              ws.Range("K" & ticker_row).NumberFormat = "0.00%"
   
              ticker_row = ticker_row + 1

              totalvolume = 0

              open_price = ws.Cells(i + 1, 3)
            
            Else
              

              totalvolume = totalvolume + ws.Cells(i, 7).Value

            
            End If
        
        Next i

    lastrow_summary_table = ws.Cells(Rows.Count, 9).End(xlUp).Row
    

        For i = 2 To lastrow_summary_table
            
            If ws.Cells(i, 10).Value > 0 Then
                ws.Cells(i, 10).Interior.ColorIndex = 4
            
            Else
                ws.Cells(i, 10).Interior.ColorIndex = 3
            
            End If
        
        Next i


        ws.Cells(2, 15).Value = "Greatest % Increase"
        ws.Cells(3, 15).Value = "Greatest % Decrease"
        ws.Cells(4, 15).Value = "Greatest Total Volume"
        ws.Cells(1, 16).Value = "Ticker"
        ws.Cells(1, 17).Value = "Value"

        For i = 2 To lastrow_summary_table
        
            If ws.Cells(i, 11).Value = Application.WorksheetFunction.Max(ws.Range("K2:K" & lastrow_summary_table)) Then
                ws.Cells(2, 16).Value = ws.Cells(i, 9).Value
                ws.Cells(2, 17).Value = ws.Cells(i, 11).Value
                ws.Cells(2, 17).NumberFormat = "0.00%"

            ElseIf ws.Cells(i, 11).Value = Application.WorksheetFunction.Min(ws.Range("K2:K" & lastrow_summary_table)) Then
                ws.Cells(3, 16).Value = ws.Cells(i, 9).Value
                ws.Cells(3, 17).Value = ws.Cells(i, 11).Value
                ws.Cells(3, 17).NumberFormat = "0.00%"
            
            ElseIf ws.Cells(i, 12).Value = Application.WorksheetFunction.Max(ws.Range("L2:L" & lastrow_summary_table)) Then
                ws.Cells(4, 16).Value = ws.Cells(i, 9).Value
                ws.Cells(4, 17).Value = ws.Cells(i, 12).Value
            
            End If
        
        Next i
    
    Next ws
        
End Sub

