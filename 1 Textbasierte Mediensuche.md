# a) Teilen von Texten

Erstellen Sie eine Funktion teile, die ein String als Eingabe erwartet, diesen in seine einzelnen Terme aufteilt und in einem Cell-Array zurückgibt. Die zum Trennen verwendeten Zeichen werden als Parameter Trennzeichen an die Funktion übergeben.

### Input:
Dokument - String, der einen Text enthält
Trennzeichen - Vektor aus Char-Elementen (≜ String). Jedes Element entspricht einem Zeichen bei dem das Dokument geteilt werden soll

### Output:
Terme - Cell-Array der einzelnen Wörter

### Hinweise:
Die Verwendung der Matlab-Funktionen split(), strsplit(), etc. ist nicht erlaubt
Trennzeichen können auch mehrfach hintereinander im Dokument vorkommen
Das Dokument kann sowohl mit als auch ohne Trennzeichen beginnen/enden
nützliche Funktionen: strlength(), ismember()


```matlab
function Terme = teile( Dokument, Trennzeichen )
%% Funktion, welche Dokument in seine einzelnen Terme aufteilt

    % Initialize variables初始化变量
    Terme = {};        % empty array
    akt_term = '';     % empty string
    
    % Iterate through the characters of the document string逐字符遍历文档字符串
    for i = 1:strlength(Dokument)
        akt_Char = Dokument(i);
        
        % Check if the current character is a delimiter检查当前字符是否为分隔符
        if ismember(akt_Char, Trennzeichen)
            % Add the current term to the cell array将当前项添加到单元格数组中
            if ~isempty(akt_term)
                Terme = [Terme, {akt_term}];
                akt_term = '';
            end
        else
            % Add the character to the current term将字符添加到当前项中
            akt_term = [akt_term, akt_Char];
        end
    end
    
    % If there is a last term, add it to the cell array如果存在最后一个项，则添加到单元格数组中
    if ~isempty(akt_term)
        Terme = [Terme, {akt_term}];
    end

end
```

## 调用函数代码
```matlab
Trennzeichen = ['.',' ','/',',','"','+',':','-','<','>','!','?'];
Dokument = 'The Test-sTRing, whiCh is the best for "tesTing" things';
Terme = teile( Dokument, Trennzeichen )
```

## output
```matlab
Terme =

  1×10 cell array

    {'The'}    {'Test'}    {'sTRing'}    {'whiCh'}    {'is'}    {'the'}    {'best'}    {'for'}    {'tesTing'}    {'things'}
```
