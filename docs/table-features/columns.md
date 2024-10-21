# Table Columns

This section covers the features of your Table Component Columns.

Here you will find:

[[toc]]

## Add a Column

See [Include columns](/table-component/component-columns.html#include-columns).

## Configure a Column

See [Column Configuration Methods](/table-component/component-columns.html#column-configuration-methods).

## Show/Hide Columns

See [Toggle Column Visibility](/table-features/header-and-footer.html#toggle-column-visibility) to enable a button giving power to the user to hide and show columns.

Additionally, see the method [`Column::hide()`](/table-component/component-columns.html#hidden) to programmatically hide/show a specific column.

Furthermore, you may see [Exclude Columns From Exporting](/table-features/exporting-data.html#exclude-columns-from-exporting) if you need to omit certain Columns when exporting data.

## Action Column

The Action Column is a dedicated column to display Row Actions such as [Buttons](/table-features/rows.html#buttons) and [Actions From View](/table-features/rows.html#actions-from-view).

To include an Action Column, just add a call to `Column::action()` in your Component's `columns()` method.

Typically, the Action Column is the last column in your Table, however may add this column as many times as you want and at any desired position.
If you deal with export, by default Action Column are not included but the method [`Column::visibleInExport()`](/table-component/component-columns.html#visibleinexport) can be used if you need them in your exported file.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
        return [
            Column::make('ID', 'id'),
            Column::make('Name', 'name'),
            Column::action('Action'), // [!code ++]
        ];
    }
}
```

## Edit On Click

Edit on click allows editing the cell content directly in the row, without leaving the Table.

Clicking on the cell will convert it into a `text input`. The user can type a new text and submit changes by pressing the `<enter>` key. To discard changes, the user can press the `<ESC>` key.

If the record has a `null` value, a fallback can be displayed instead.

To enable this feature, just chain the `->editOnClick()` method to a `LivewirePowerGrid\Column` class. Inline editing is not available when using `Collection` Data Source.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
        return [
            Column::add()
                ->title('Name')
                ->field('name'),
                ->editOnClick(// [!code ++:4]
                    hasPermission: auth()->check(),
                    fallback: '- empty -'
                ),
        ];
    }
}
```

The `editOnClick()` method accepts the following parameters:

| Parameter              | Description                                                  | Default |
|------------------------|--------------------------------------------------------------|---------|
| (bool) $hasPermission  | When `true`, it enables  "edit on click" .                   | `true`  |
| (?string) $fallback    | Fallback text for `null` values.                              | `null`  |
| (bool) $saveOnMouseOut | If `true`, submit changes when clicking outside the text input. | `false`   |

::: tip 💡 TIP
Learn more about how to update data from [Edit On Click](/table-features/updating-data.html#edit-on-click).
:::

## Toggleable

Toggle buttons provide a convenient way to display the `boolean` state of a record, while also giving the possibility to quickly switch between these values.

When using this feature, the cell will be converted into a toggleable button. If the user does not have permission to edit data, a fallback label will be displayed instead.

To enable this feature, just chain the `->toggleable()` method to a `LivewirePowerGrid\Column` class. Toggle switch is not available when using `Collection` Data Source.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
        return [
            Column::add()
                ->title('In Stock')
                ->field('in_stock')
                ->toggleable(// [!code ++:5]
                        hasPermission: auth()->check(),
                        trueLabel: 'Yes',
                        falseLabel: 'No'
                ),
        ];
    }
}
```

The `toggleable()` method accepts the following parameters:

| Parameter              | Description                 | Default |
|------------------------|-----------------------------|---------|
| (bool) $hasPermission  | enable/disable this feature | `true`  |
| (string) $trueLabel    | Label when record is `true` | Yes     |
| (string) $falseLabel   | Label when record is `false`| No      |

::: tip 💡 TIP
Learn more about how to update data from [Toggle Switches](/table-features/updating-data.html#toggleable-switch).
:::

## Column Summary

PowerGrid can display each column's sum, count, average, minimum, and maximum value inside column headers.

Summaries are chained to the [Component Column](/table-component/component-columns.html) methods `Column::add()` or `Column::make()`.

### Sum

Display the sum of all records in the column.

All data in your database will be preprocessed to fetch the sum of all records. Only one request will be made when using `->get()`.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
         return [
            Column::make('Price', 'price')// [!code ++:2]
                ->withSum('Sum Price', header: true, footer: false)
          ];
    }
}
```

| Parameter       | Description                                                               | Default |
|-----------------|---------------------------------------------------------------------------|---------|
| (string) $label | The argument $label sets the summary caption.                              | 'Sum'   |
| (bool) $header  | If `true`, results will be displayed in the Table header, under the filters. | `false`  |
| (bool) $footer  | If `true`, results will be displayed in the Table footer.     | `false`  |

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Sum</a>.</p>

</div>

---

### Count

Display the count of all records in the column.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
         return [
            Column::make('Price', 'price')// [!code ++:2]
                ->withCount('Count Price', header: true, footer: false)
          ];
    }
}
```

| Parameter       | Description                                                               | Default |
|-----------------|---------------------------------------------------------------------------|---------|
| (string) $label | The argument $label sets the summary caption.                              | 'Count' |
| (bool) $header  | If `true`, results will be displayed in the Table header, under the filters. | `false`  |
| (bool) $footer  | If `true`, results will be displayed in the Table footer.     | `false`  |

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Count</a>.</p>

</div>

---

### Average

Display the average value of all records in the column.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
         return [
            Column::make('Price', 'price')// [!code ++:2]
                ->withAvg('Avg Price', header: true, footer: false)
          ];
    }
}
```

| Parameter       | Description                                                               | Default |
|-----------------|---------------------------------------------------------------------------|---------|
| (string) $label | The argument $label sets the summary caption.                             | 'Avg'   |
| (bool) $header  | If `true`, results will be displayed in the Table header, under the filters. | `false`  |
| (bool) $footer  | If `true`, results will be displayed in the Table footer.     | `false`  |

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Average</a>.</p>

</div>

---

### Min

Display the minimum value of records in the given column.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
         return [
            Column::make('Price', 'price')// [!code ++:2]
                ->withMin('Min Price', header: true, footer: false)
          ];
    }
}
```

| Parameter       | Description                                                               | Default |
|-----------------|---------------------------------------------------------------------------|---------|
| (string) $label | The argument $label sets the summary caption.                              | 'Min'   |
| (bool) $header  | If `true`, results will be displayed in the Table header, under the filters. | `false`  |
| (bool) $footer  | If `true`, results will be displayed in the Table footer.     | `false`  |

---

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Min</a>.</p>

</div>

---

### Max

Display the maximum value of records in the given column.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use PowerComponents\LivewirePowerGrid\Column;

class DishTable extends PowerGridComponent
{
   public function columns(): array
    {
        return [
            Column::make('Price', 'price')// [!code ++:2]
                ->withMax('Max Price', header: true, footer: false)
        ];
    }
}
```

| Parameter       | Description                                                               | Default |
|-----------------|---------------------------------------------------------------------------|---------|
| (string) $label | The argument $label sets the summary caption.                              | 'Max'   |
| (bool) $header  | If `true`, results will be displayed in the Table header, under the filters. | `false`  |
| (bool) $footer  | If `true`, results will be displayed in the Table footer.     | `false`  |

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Max</a>.</p>

</div>

## Formatting Summarized Data

To format the summarized data, you must add the `summaryFormat()` returning an associative array containing `column`.{`summary methods`},  and a `closure` to format data. You must provide the summary methods comma separated.

Summary methods are: `sum`, `avg`, `count`, `min`, `max`.

Example:

```php
// app/Livewire/DishTable.php

use PowerComponents\LivewirePowerGrid\PowerGridComponent;
use Illuminate\Support\Number;

class DishTable extends PowerGridComponent
{
  public function summarizeFormat(): array// [!code ++:8]
  {
    return [
        'price.{sum,avg,min,max}' => fn ($value) => Number::currency($value, in: 'USD'),
        'price.{count}'  => fn ($value) => Number::format($value, locale: 'br'),
        'calories.{avg}' => fn ($value) => Number::format($value, locale: 'br', precision: 2) . ' kcal',
    ];
  }
}
```

<div class="onlinedemo custom-block">
  <p class="custom-block-title">🚀 See it in action</p>
  <p>See an interactive example using <a target="_blank" href="https://demo.livewire-powergrid.com/examples/summarize">Formatting Summarized Data</a>.</p>

</div>
