# **Guía de estilo de XAML (Xamarin.Forms/Maui)**

# Declinación de responsabilidades
Al pasar esta guía a escrito pueden surgir contradicciones, inexactitudes y aspectos no tenidos en cuenta entre otras cosas. Durante años no hemos necesitado tenerla por escrito para hacer un código limpio y similar entre nosotros. Confía en tu instinto y piensa en como te gustaría encontrarte el código si llegases nuevo al proyecto.

Esta guía se presenta **únicamente como guía de orientación** y para plasmar el estilo de XAML que durante años hemos usado algunos compañeros y yo. Siéntate antes con tu equipo y hablad que estilo, normas o reglas váis a seguir. 

# Motivación

## No romper nada.
Con esta guía de estilo no pretendo romper algunas convenciones *no escritas* que tiene el XAML (Como el uso de una linea por cada namespace en el objeto principal del archivo). Sino recoger y escribir las normas comunes y añadir aquellas para las que nada se ha establecido.

**Y por supuesto, no dudes en abrir una *issue* o presentar un *pull request* si crees que se puede mejorar.**

## Guías oficiales.
Uno de los motivos para escribir está guía es que no he encontrado guías "*oficiales*" o "*standards*" para seguir un estándar apropiado para escritura de XAML al diseñar pantallas o controles en **Xamarin.Forms** o **Maui**. 

No hay tales indicaciones explicitas para el XAML en los GitHubs oficiales de **Xamarin.Forms** o **Maui**, sino que sólo indicaciones basadas en las [reglas de la .NetFoundation](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md) para C#.

En el GitHub oficial de **Xamarin.Forms** se limita a seguirlas con 3 excepciones que explican aquí: [Coding style en Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms#coding-style).

![Captura del estilo de código de Xamarin.Forms](https://user-images.githubusercontent.com/10408239/183343188-a9427f3b-ce82-4c0b-a532-aabd2f877092.png)

En [el caso de Maui](https://github.com/dotnet/maui/blob/main/.github/CONTRIBUTING.md#what-to-work-on), es aún más reducido y lo han limitado a las dos primeras excepciones que se explican en el Xamarin.Forms.

![Captura del estilo de código de Maui](https://user-images.githubusercontent.com/10408239/183343089-198ea1b7-b8d1-4fd7-a4fc-d9e9d39a3668.png)

De las tres mostradas, la primera de ellas es ya común con XAML, ya que no se suele añadir [`x:FieldModifier="Private"`](https://docs.microsoft.com/es-es/dotnet/maui/xaml/field-modifiers) en nuestros controles, pero las otras dos indicadas para **Xamarin.Forms** las adoptaremos y explicaremos en nuestra guía de estilo.

## La Realidad.
Aunque a todos consideramos que a la hora de escribir llevamos cierto sentido práctico y organizado, lo cierto es que, aunque sea así, puede no ser el mismo sentido práctico y organizado que el del resto de miembros del equipo. Por lo que en esta guía se plasmarán las ideas principales a seguir para que independientemente de qué miembro del equipo desarrolle el código, se escriba un XAML similar.

# Reglas.

## Tabulaciones antes que espacios.
Como se comentó anteriormente, es una de las reglas de Xamarin.Forms y Maui. En [el enlace de Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms#coding-style) podréis encontrar como configurar Visual Studio para que lo haga de forma automática.

Pensad que para alguien que tenga la accesibilidad activa, es mejor escuchar dos veces la palabra *tab* que ocho veces la palabra *space*.

## Saltos de línea.
En XAML, heredando el estilo del diseñador de *WPF* de Visual Studio, se ha estilado el ir colocando propiedades y, cuando cada uno considere, introducir un salto de línea. Ya se ha explicado que la idea de esta guía [no es romper esquemas establecidos](#no-romper-nada), sino establecer reglas de cuando usarlos. Para ellos veamos cuando introducir un salto de linea en la definición de nuestro control en XAML.

1. ### Por tipo de propiedad.
Vamos a agrupar las propiedades en tipos y usaremos esos mismos tipos para saber en qué línea estará colocada cada propiedad del control. Los tipos para cada línea y el orden que deben llevar [se especifican más abajo](#orden).

Ejemplo:
```XAML
<Label x:Name="LblName" AutomationId="LblName"
       Text="{Static r:Text.FieldName}" FontSize="Body"
       BackgroundColor="Green"
       TranslateX="15" HorizontalOption="Center" />
```
En este ejemplo vemos como en la primera línea hemos colocado cualquier propiedad que nos pueda ayudar a identificar el control (x:Key, x:Name, etc...), en la segunda línea propiedades más o menos exclusivas del control (Texto, fuente, tamaño de letra, etc...), en la tercera línea propiedades más o menos comunes al resto de controles que pueda haber en nuestro documento (Color de fondo, opacidad, etc...) y en último lugar, todo lo referente a la posición.

2. ### Longitud de linea.
El *Coding Style* de **Xamarin.Forms** establece que la longitud de línea debe limitarse a 120 lo máximo posible. En ocasiones en las que nos saltemos demasiado ese límite por el número de propiedades del mismo tipo o la longitud de las mismas, podremos usar dos o más líneas para ello.

Ejemplo:
```XAML
<Button Text="{Static r:Text.BtnSearch}" FontSize="Body"
        Opacity="{Binding UserEnabledSearchSelection, Converter={StaticResource PercentConverter}"
        IsEnabled="{Binding UserEnabledSearchSelection, Converter={StaticResource IntToBoolConverter}}"
        Margin="5,10,5,0" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
```
En este ejemplo, dado que *Opacity* e *IsEnabled* tienen demasiada longitud debido a sus bindings, hemos usados dos líneas para los tipos comunes.

## Orden
1. ### Argumentos, diccionarios y propiedades
Lo primero que debemos colocar dentro de la definición del control son las propiedades intrínsecas expandidas del control, así como aquellas que son heredadas de un padre, dejando las más comunes (*.Content*, *.Children*, *.ItemTemplate*, etc...) para las últimas. De esta forma si un nuevo desarrollador quiere visualizar el contenido, antes verá que hay ciertas condiciones especiales.

Por ejemplo, en un *ContentPage*, si alguien desea ver el contenido, antes vería que ese contenido tiene ciertos *triggers* que pueden afectar al diseño, antes de ver el contenido del diseño de la pantalla.

```XAML
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
	     xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
	     x:Class="MyProject.App.Features.Dashboard.DashboardPage">
	<ContentPage.Triggers>
		...
	</ContentPage.Triggers>
	<ContentPage.Content>
		...
	</ContentPage.Content>
</ContentPage>
```

2. ### Organización de las propiedades dentro del control.

Organización de las distintas propiedades por líneas.

   1. #### Identificadores.
Son las propiedades que nos ayudan a identificar el control. Las ponemos en primer lugar para poder identificar rápidamentel control y su funcionalidad. 

Ejemplos: `x:Name`, `x:Key`, `AutomationId`, etc...


   2. #### Estilos.
En segundo lugar ponemos el estilo del control, por el mismo motivo que poníamos los argumentos y diccionarios en primer lugar. Al ver que un control tiene un estilo asignado, sabemos que además de las propiedades que pudiéramos ver a continuación, puede tener otras ocultas en el estilo.

También esta propiedad puede ayudarnos a darnos una idea general del control identificando su tipo por su estilo incluso aunque no tuviera identificadores. Por ejemplo: Un `label` con el `Style` asingado `LabelHeaderStyle` nos indicaría que es una etiqueta destacada en la pantalla.

Ejemplos: `Style`, `StyleClass`
```XAML
<Label Style="{StaticResource LabelHeaderStyle}"
       ... />
```

`Style` pierde su ayuda para identificar cuando ya el control tiene su primera propiedad que la identifique, por lo que lo ideal es añadirlo en la primera línea **después** de la propiedad identificadora.
Ejemplos:
```XAML
<Label x:Key="LblTitleSection" Style="{StaticResource LabelHeaderStyle}"
       ... />
```
```XAML
<Label AutomationId="LabelTitleSection" Style="{StaticResource LabelHeaderStyle}"
       ... />
```

   3. #### Propiedades del padre.
Después de la identificación, añadimos las propiedades que nos vengan impuestas de un control superior. Por ejemplo:

```XAML
<Label x:Key="LblTitleSection" Style="{StaticResource LabelHeaderStyle}"
       Grid.Row="1" Grid.ColumnSpan="2"
       ... />
```

   4. #### Propiedades propias del control.
En la parte central del control añadimos el resto de propiedades que no sean de posición. Aunque aquí es donde quedará la parte más "desorganizada", por lo que intentemos seguir un patrón de relación, poniendo propiedades relacionadas o con nombres similar juntas.

Ejemplo:
```XAML
<Label x:Name="LblName" AutomationId="LblName" Style="{StaticResource LabelDefaultStyle}"
       Grid.Row="1" Grid.ColumnSpan="2"
       Text="{Static r:Text.FieldName}" TextColor="{StaticResource RedColor}" BackgroundColor="Green"
       FontAttributes="Bold" FontSize="Body" HorizontalTextOption="Center"
       ... />
```
En este ejemplo podemos ver que agrupamos en la línea 3 y 4 las propiedades comunes, y entre ellas, agrupamos las propiedades `Text...` y `Font...` en la misma línea. Además, hemos podido colocar las propiedades referentes al color juntas también.

   5. #### Diseño del control.
Aunque son propiedades que bien podrían agruparse junto a las del punto 4, si agrupamos las propiedades que afecten al diseño, bordes, etc... nos será más fácil localizarlas el día que necesitemos un cambio de diseño


Ejemplo:
```XAML
<Frame Grid.Row="1"
       BackgroundColor="Green"
       CornerRadious="25" HeightRequest="50"
       ... />
```

   6. #### Posición del control.
Al final del control colocaremos las propiedades referentes a la posición del control.

Ejemplo:
```XAML
<Label x:Name="LblName" AutomationId="LblName" Style="{StaticResource LabelDefaultStyle}"
       Grid.Row="1" Grid.ColumnSpan="2"
       Text="{Static r:Text.FieldName}" TextColor="{StaticResource RedColor}" BackgroundColor="Green"
       FontAttributes="Bold" FontSize="Body" HorizontalTextOption="Center"
       TranslateX="25" WidthRequest="250"
       HorizontalOptions="EndAndExpand" VerticalOptions="Center" />
```


## Nada está escrito en piedra pero ten sentido común.

Hasta el día de hoy, todas y cada una de las reglas, guías de estilo o estilos de código, han tenido sus excepciones de cuando no usarlas sin que ello conlleve la destrucción del universo conocido.

> Hasta la regla del código limpio y legible puedes saltártela cuando le haces un **minify** a un fichero o si **ofuscas** el código justo antes de compilarlo.

Con esto no quiero decir que puedas obviar las reglas sin razón ni motivo, sino bajo justificación. **La idea principal de esta guía de estilo es facilitar la lectura del XAML**, no imponer reglas estrictas.

También debemos tener en cuenta que en esta guía se confía en el juicio del programador. Hay propiedades, como `Margin` o `HeightRequest` que podrían considerarse parte de las propiedades de diseño o de las de posición. Está en el juicio del programador colocarla dónde mejor encaje en cada situación sin que dificulte su lectura.

Ejemplos:
```XAML
<Label Text="{Static r:Text.FieldName}" TextColor="{StaticResource RedColor}" BackgroundColor="Green"
       Margin="15,25,15,0" Padding="5,10" TranslateX="25"
       HorizontalOptions="EndAndExpand" VerticalOptions="Center" />
```
```XAML
<Label Text="{Static r:Text.FieldName}" TextColor="{StaticResource RedColor}" BackgroundColor="Green"
       Margin="15,25,15,0" VerticalOptions="Center" />
```

# Variantes de la guía de estilo.

Diferentes variantes propuestas.

## Regla de los saltos de linea.
En algunos equipos puede resultar extraño líneas de distinta longitud si no están acostumbrados a ello, por lo que **respetando el orden**, se podría eliminar el salto de línea por categoría y [mantener únicamente el de Xamarin.Forms](#guías-oficiales).

Ejemplos:
```XAML
<Label x:Name="LblName" Grid.Row="1" Grid.ColumnSpan="2" Text="{Static r:Text.FieldName}"
       TextColor="{StaticResource RedColor}" BackgroundColor="Green" FontAttributes="Bold"
       FontSize="Body" HorizontalTextOption="Center" TranslateX="25" WidthRequest="250"
       HorizontalOptions="EndAndExpand" VerticalOptions="Center" />
```
```XAML
<Label x:Name="LblName" Grid.Row="1" Text="{Static r:Text.FieldName}"
       HorizontalOptions="EndAndExpand" VerticalOptions="Center" />
```
```XAML
<Frame Grid.Row="1" BackgroundColor="Green" CornerRadious="25" HeightRequest="50" />
```
