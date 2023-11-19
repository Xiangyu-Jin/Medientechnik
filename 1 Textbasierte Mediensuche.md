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

    % Initialize the normalized cell array初始化规范化后的单元格数组
    nTerme = [];

    % Iterate through each term in the input cell array遍历输入单元格数组中的每个术语
    for i = 1:numel(Terme)
        new_term = lower(Terme{i});  % Convert the current term to lowercase将当前术语转换为小写

        % Suche im Wörterbuch nach dem aktuellen Term在词典中查找当前术语
        index = find(strcmpi(new_term, WBuch(:, 1)));

        % If the term is found in the dictionary, replace it with the corresponding base form如果在词典中找到术语，则用对应的基本形式替换
        if ~isempty(index)
            normalizedTerm = lower(WBuch{index, 2});
            nTerme = [nTerme, {normalizedTerm}];
        else
            % If the term is not found in the dictionary, keep it as is如果在词典中未找到术语，则保留原样
            nTerme = [nTerme, {new_term}];
        end
    end
    
end
```

## calls the function
```matlab
WBuch = {  'am', 'be';...
          'is','be'; ...
          'were','be';...
          'testing','test';...
          'best','good';...
          'Things','thing'};
                
Terme = {'The','Test','sTRing','whiCh','is','the','best','for','tesTing','things'};

nTerme = normalisieren(Terme, WBuch)
```

## output
```matlab
nTerme =

  1×10 cell array

    {'the'}    {'test'}    {'string'}    {'which'}    {'be'}    {'the'}    {'good'}    {'for'}    {'test'}    {'thing'}
```



# c) Filtern von Stopwörtern
Erstellen Sie die Funktion mein_Filter, die aus einem Cell-Array alle Stoppwörter entfernt. Die Liste der zu entfernenden Stoppwörter wird hierbei als zweiter Parameter in Form eines weiteren Cell-Arrays übergeben.

### Input:
nTerme - Cell-Array der normalisierten Terme
stoppWort - Cell-Array, welches alle Stoppwörter enthält

### Output:
fTerme - Cell-Array der gefilterten Terme

### Hinweis:
Die Stoppwörter sind bereits normalisiert
nützliche Funktionen: strcmp(), strcmpi(), numel()
这里是这些 MATLAB 函数的简要说明：
1. `strcmpi(new_term, stoppWort)`: 使用 `strcmpi` 函数比较 `new_term` 与 `stoppWort` 中的每个元素是否相等，忽略大小写。该函数返回一个逻辑数组，指示每个元素是否相等。
2. `any(...)`: `any` 函数用于检查逻辑数组中是否存在至少一个 `true` 值。如果在比较中有任何一个元素相等（即 `true`），则 `any` 返回 `true`，否则返回 `false`。

```matlab
function [ fTerme ] = mein_Filter( nTerme, stoppWort )
%% Funktion, welche Stoppwörter rausfiltert

    % Initialize the filtered cell array
    fTerme = [];

    % Iterate through the input normalized terms cell array遍历输入的规范化后的术语单元格数组
    for i = 1:numel(nTerme)
        new_term = nTerme{i};

        % Check if the current term is a stop word检查当前术语是否为停用词
        isStopWord = any(strcmpi(new_term, stoppWort));    % isStopWord =0/1 logicwert -

        % If it's not a stop word, add the term to the filtered cell array如果不是停用词，将术语添加到过滤后的单元格数组中
        if ~isStopWord
            fTerme = [fTerme, {new_term}];
        end
    end

end
```

## calls the function
```matlab
stoppWort = {'be', 'to', 'in', 'the', 'and'};
nTerme = {'the','test','string','which','be','the','good','for','test','thing'};

fTerme = mein_Filter( nTerme, stoppWort )
```

## output
```matlab
fTerme =

  1×7 cell array

    {'test'}    {'string'}    {'which'}    {'good'}    {'for'}    {'test'}    {'thing'}
```



# d) Indizierung
Erstellen Sie die Funktion indiziere, die einen erwiterten "inverted index" für alle N Dokumente erstellt. Der Index soll als ein "containers.Map" implementiert werden. Dabei soll für jedes normalisierte Wort ein Vektor der größe 1xN erstellt werden in dem jedes Element angibt wie oft das Normalisierte Wort im n Dokument vorkommt.
Sie können hierfür die Funktionen, welche in den vorherigen Aufgaben entwickelt wurden verwenden. Sie funktionieren, auch wenn Sie diese vorher nicht selber lösen konnten.

### Input:
Dokumente - Cell-Array aus einzelnen Dokumenten (Strings)
Trennzeichen - Vektor aus Char-Elementen (≜ String). Jedes Element entspricht einem Zeichen bei dem das Dokument geteilt werden soll
WBuch - Wörterbuch als zweidimensionales Cell-Array
stoppWort - Cell-Array, welches alle Stoppwörter enthält

### Output:
invertedIndex - ein containers.Map, welches für jedes indizierte Wort einen Zeilenvektor enthält, der angibt in welchen Dokument es wie oft vorkommt

### Hinweis:
verwenden Sie die vorherigen Funktionen mein_Filter, normalisieren und teile
den invertierten Index können sie mit containers.Map(...)indizieren
nützliche Funktionen: numel(), isKey()

```matlab

```

## calls the function
```matlab
load('dokumente.mat');
Trennzeichen = ['.',' ','/',',','"','+',':','-','<','>','!','?'];
WBuch = {  'ist', 'sein';...
         'war','sein'};
stoppWort = {'sein', 'der', 'die', 'das', 'und'};

invertedIndex = indiziere( Dokumente, Trennzeichen, WBuch, stoppWort );
invertedIndex.keys
```

## output
```matlab

```



# 


```matlab

```

## calls the function
```matlab

```

## output
```matlab

```



# 


```matlab

```

## calls the function
```matlab

```

## output
```matlab

```



# 


```matlab

```

## calls the function
```matlab

```

## output
```matlab

```
