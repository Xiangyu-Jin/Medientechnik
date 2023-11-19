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

## calls the function
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



# b) Normalisieren

Schreiben Sie eine Funktion normalisieren, die für jeden Term eines Cell-Arrays seine Grundform findet. Der Rückgabewert soll folglich ein Cell-Array mit Wörtern in ihrer Grundform sein. Hierür wird ein Wörterbuch benötigt. Dieses wird als weiterer Eingabeparamter in Form eines Nx2 großen Cell-Arrays übergeben. Dabei stehen in der ersten Spalte die Wörter, die durch die Wörter in der zweiten Spalte ersetzt werden sollen. Zudem sollen alle normalisierten Wörter nur aus Kleinbuchstaben bestehen.

### Input:
Terme - Cell-Array aus einzelnen Wörter
WBuch - Wörterbuch als zweidimensionales Cell-Array

### Output:
nTerme - Cell-Array der normalisierten Terme

### Hinweise:
Klein- und Großschreibung soll im Wörterbuch nicht beachtet werden
Nützliche Funktionen: lower(), strcomp(), strcompi(), numel(), size()
这里是这些 MATLAB 函数的简要说明：

1. `lower()`: `lower` 函数用于将字符串转换为小写形式。例如，`lower('Hello')` 返回字符串 `'hello'`。

2. `strcomp()`: `strcomp` 函数用于比较两个字符串是否相等。该函数返回逻辑值，如果字符串相等，则为 `1`，否则为 `0`。

3. `strcompi()`: `strcompi` 函数是 `strcomp` 的不区分大小写版本，用于比较两个字符串是否相等，忽略大小写。

4. `numel()`: `numel` 函数用于返回数组中元素的数量。对于矩阵，它返回矩阵的总元素数。

5. `size()`: `size` 函数用于返回数组的大小。对于矩阵，它返回一个包含行数和列数的向量。例如，`size(matrix)` 返回一个包含行数和列数的向量。


```matlab
function nTerme = normalisieren(Terme, WBuch)
%% Funktion, welche Terme normalisiert

end
```
