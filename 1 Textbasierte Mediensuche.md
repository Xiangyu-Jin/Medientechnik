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
function [invertedIndex] = indiziere(Dokumente, Trennzeichen, WBuch, stoppWort)
%% indiziere - 创建文档集合的倒排索引
%% Funktion, welche für eine Dokumentensammlung einen Inverted Index erstellt

    % 初始化倒排索引为一个 containers.Map 对象
    % Initialize the inverted index as a containers.Map
    invertedIndex = containers.Map;

    % 遍历输入的文档单元格数组中的每个文档
    % Iterate through each document in the input cell array
    for docIndex = 1:numel(Dokumente)
        document = Dokumente{docIndex};

        % 将文档拆分为词语
        % Split the document into terms
        terme = teile(document, Trennzeichen);

        % 对词语进行标准化并过滤停用词，参考任务 b) 和 c)
        % Normalize and filter stop words from the terms. Aufgabe b) & c)
        normalizedTerme = normalisieren(terme, WBuch);
        filteredTerme = mein_Filter(normalizedTerme, stoppWort);

        % 遍历每个过滤后的词语并更新倒排索引
        % Iterate through each filtered term and update the inverted index
        for termIndex = 1:numel(filteredTerme)
            new_term = filteredTerme{termIndex};

            % 如果词语已经在倒排索引中，更新向量
            % If the term is already in the inverted index, update the vector
            if invertedIndex.isKey(new_term)  % true/false
                new_vector = invertedIndex(new_term);
                new_vector(docIndex) = new_vector(docIndex) + 1;
                invertedIndex(new_term) = new_vector;
            else
                % 如果词语不在倒排索引中，添加新条目
                % If the term is not in the inverted index, add it
                new_vector = zeros(1, numel(Dokumente));
                new_vector(docIndex) = 1;
                invertedIndex(new_term) = new_vector;
            end
        end
    end
    
end

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
ans =

  1×35 cell array

  Columns 1 through 14

    {'also'}    {'another'}    {'auch'}    {'auf'}    {'be'}    {'beispiel'}    {'can'}    {'dieser'}    {'documents'}    {'ein'}    {'eine'}    {'exzellenzunivers…'}    {'hier'}    {'ihre'}

  Columns 15 through 28

    {'im'}    {'in'}    {'kein'}    {'konnte'}    {'language'}    {'lmt'}    {'praktikum'}    {'rwth'}    {'satz'}    {'schaukel'}    {'schwarz'}    {'schwinguin'}    {'sitzt'}    {'stehen'}

  Columns 29 through 35

    {'the'}    {'tum'}    {'was'}    {'weiss'}    {'werbung'}    {'willkommen'}    {'written'}
```



# e) Boolesche Suche
Erstellen Sie die Funktion boolescheSuche, welche für eine gegebene Suchanfrage Dokumente findet, welche die Suche erfüllen. Die Suchanfrage besteht aus einem Cell-Array, welches wiederum zwei Cell-Arrays beinhaltet. Das erste Array enthält Wörter, die im Dokument vorkommen sollen, und das Zweite Wörter, die im Dokument nicht vorkommen sollen (siehe "Code to call your function").
Erstellen Sie hierzu als erstes eine Indzidenzmatrix m, welche angibt in welchen Dokumenten welche Wörter vorkommen. 
Anschließend wenden sie die richtigen booleschen Operatoren auf dei Matrix an.

### Input:
suche - Cell-Array bestehend aus zwei Cell-Arrays, welche Terme enthalten, die (nicht) vorkommen sollen
invertedIndex - ein containers.Map, welches für jedes indizierte Wort einen Vektor enthält, der angibt in welchen Dokument es vorkommt

### Output:
m - Inzidenzmatrix als boolesche Matrix der größe |Terme| x |Dokumente|
dokumenteIds - Vektor mit allen Dokumente IDs, die die Suche erfüllen

### Hinweis:
Die boolesche Suche ist im Skript ab Seite 6 beschrieben
Sie können wieder die zuvor implementierten Funktionen benutzen
gehen Sie davon aus, dass die Suche bereits normiert ist und keine Stoppwörter enthält
Die Indzidenzmatrix kann sehr einfach aus dem inverted index erstellt werden
nützliche Funktionen: logical(), contains(), find()

```matlab
function [ dokumenteIds, m ] = boolescheSuche( suche, invertedIndex )
%% Funktion, welche eine boolesche Suche ausführt

% Holen eindeutige Begriffe aus dem invertedIndex
terms = keys(invertedIndex);

% Initialisieren die Inzidenzmatrix mit Nullen
m = zeros(length(terms), numel(invertedIndex(terms{1})));
satisfies = ones(1, numel(invertedIndex(terms{1})));

% Füllen die Inzidenzmatrix basierend auf dem invertedIndex
for termIndex = 1:numel(terms)
    term = terms{termIndex};
    m(termIndex, :) = invertedIndex(term);
end

% umwandeln in logischen Typ 
m = logical(m);

% Extrahieren die zu "soll" und "nicht" Begriffe
suche_soll = suche{1};
suche_nicht = suche{2};

% Wenden boolesche Operatoren auf die Inzidenzmatrix an:
% soll
for i = 1:numel(suche_soll)
    term_soll = suche_soll{i};
    if isKey(invertedIndex, term_soll)
        term_vector_soll = m(contains(terms, term_soll), :);
        satisfies = satisfies & term_vector_soll;
    end
end

% nicht
for j = 1:numel(suche_nicht)
    term_nicht = suche_nicht{j};
    if isKey(invertedIndex, term_nicht)
        term_vector_nicht = m(contains(terms, term_nicht), :);
        satisfies = satisfies & ~term_vector_nicht;
    end
end

end
```

## calls the function
```matlab
load('dokumente.mat');
trennzeichen = ['.',' ','/',',','"','+',':','-','<','>','!','?'];
dict = {  'ist', 'sein';...
         'war','sein'};
stoppWort = {'sein', 'der', 'die', 'das', 'und'};

invertedIndex = indiziere( Dokumente, trennzeichen, dict, stoppWort );

suche = {{'beispiel'},{}};
[ dokumenteIds, m ] = boolescheSuche(suche, invertedIndex);
dokumenteIds
```

## output
```matlab
dokumenteIds =

     2
```



# f) Freitextsuche
Erstellen Sie die Funktion freitextSuche, welche für einen beliebig langen Suchtext die Dokumente sortieret nach ihrer Ähnlichkeit zurückgibt. Verwenden Sie hierfür die Summe der tf-idf Werte.
### Input:
sucheText - beliebiger String
invertedIndex - ein containers.Map, welches für jedes indizierte Wort einen Vektor enthält, der angibt in welchen Dokument es wie oft vorkommt
Trennzeichen - Vektor aus Char-Elementen (≜ String). Jedes Element entspricht einem Zeichen bei dem das Dokument geteilt werden soll
WBuch - Wörterbuch als zweidimensionales Cell-Array
stoppWort - Cell-Array, welches alle Stoppwörter enthält
### Output:
Rang - Vektor, welcher die Ähnlichkeitswerte absteigend sortiert enthält (Höchste tf-idf Summe ≜ erstes Element)
dokumenteIds - Vektor mit allen Dokumente IDs, geordnet nach dem Rang
### Hinweis:
Die Freitextsuche ist im Skript ab Seite 7 beschrieben
Sie können wieder die zuvor implementierten Funktionen benutzen
dieses mal muss die Suchanfrage noch geteilt, normiert und gefiltert werden
bitte verwenden Sie den Logarithmus zur Basis 10 zur Berechnung der idf 
nützliche Funktionen: logical(), contains(), find()
die Matlab-Funktion tfidf() darf nicht verwendet werden

```matlab
function [ dokumenteIds, Rang ] = freitextSuche( suchText, invertedIndex, Trennzeichen, WBuch, stoppWort )
%% Funktion, welche eine Freitextsuche ausführt

    % Split
    Terme = teile(suchText, Trennzeichen);
    
    % Normalize
    norm_Terme = normalisieren(Terme, WBuch);
    
    % Filter out stop words and remove duplicates
    gefil_Terme = mein_Filter(norm_Terme, stoppWort);
    
    % Initialization
    terms = keys(invertedIndex);
    N = numel(invertedIndex(terms{1}));
    dokumenteIds = 1:N;

    % tf-idf
    Rang = zeros(1, N);
    df = zeros(numel(gefil_Terme), 1);
    
    for termIndex = 1:numel(gefil_Terme)
        if isKey(invertedIndex, gefil_Terme{termIndex})            
            tf = invertedIndex(gefil_Terme{termIndex});
            
            nonZeros = zeros(size(tf));
            nonZeros(tf ~= 0) = 1;
            
            zwi_df = sum(nonZeros);
            %zwi_df = nnz(tf);
            
            idf = log10(N / zwi_df);
            tf_idf = tf * idf;
            Rang = Rang + tf_idf;
            df(1:termIndex) = zwi_df;
        end
    end
    
    % Sort the results in descending order
    [Rang, Indizen] = sort(Rang, 'descend');
    dokumenteIds = dokumenteIds(Indizen);
    
end
```

## calls the function
```matlab
load('dokumente.mat');
Trennzeichen = ['.',' ','/',',','"','+',':','-','<','>','!','?'];
WBuch = {  'ist', 'sein';...
         'war','sein'};
stoppWort = {'sein', 'der', 'die', 'das', 'und'};

invertedIndex = indiziere( Dokumente, Trennzeichen, WBuch, stoppWort );
suchText = 'Willkommen an der Exzellenzuniversitat TUM';
[ dokumenteIds, Rang ] = freitextSuche( suchText, invertedIndex, Trennzeichen, WBuch, stoppWort )
```

## output
```matlab

Medientechnik WS23/24  1 Textbasierte Mediensuche 

 f) Freitextsuche
已提交 7 个答案 (最大数量：无限) | 查看我的答案
Die Freitextsuche ist im Skript ab Seite 7 beschrieben
Sie können wieder die zuvor implementierten Funktionen benutzen
dieses mal muss die Suchanfrage noch geteilt, normiert und gefiltert werden
bitte verwenden Sie den Logarithmus zur Basis 10 zur Berechnung der idf 
nützliche Funktionen: logical(), contains(), find()
die Matlab-Funktion tfidf() darf nicht verwendet werden
函数
更多信息
 
您答案的代码已保存。 
 
 MATLAB 文档Opens in new tab
 
function [ dokumenteIds, Rang ] = freitextSuche( suchText, invertedIndex, Trennzeichen, WBuch, stoppWort )
%% Funktion, welche eine Freitextsuche ausführt

    % Split
    Terme = teile(suchText, Trennzeichen);
    
    % Normalize
    norm_Terme = normalisieren(Terme, WBuch);
    
    % Filter out stop words and remove duplicates
…            Rang = Rang + tf_idf;
            df(1:termIndex) = zwi_df;
        end
    end
    
    % Sort the results in descending order
    [Rang, Indizen] = sort(Rang, 'descend');
    dokumenteIds = dokumenteIds(Indizen);
    
end
调用函数的代码更多信息
load('dokumente.mat');
Trennzeichen = ['.',' ','/',',','"','+',':','-','<','>','!','?'];
WBuch = {  'ist', 'sein';...
         'war','sein'};
stoppWort = {'sein', 'der', 'die', 'das', 'und'};

invertedIndex = indiziere( Dokumente, Trennzeichen, WBuch, stoppWort );
suchText = 'Willkommen an der Exzellenzuniversitat TUM';
[ dokumenteIds, Rang ] = freitextSuche( suchText, invertedIndex, Trennzeichen, WBuch, stoppWort )
 更多信息
输出
dokumenteIds =

     6     1     2     3     4     5


Rang =

    1.5563    0.7782         0         0         0         0
```




