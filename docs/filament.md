# INSTALL FILAMENT

```
composer require filament/filament
```

# INSTALL ADMIN PANEL

```
php artisan filament:install --panels
```

# MAKE USER ADMIN

```
php artisan make:filament-user
```

# ACCES ADMIN PANEL

```
http://127.0.0.1:8000/admin/login
```

# MAKE MODEL PLAYERS

```
php artisan make:model Player -m
```

# CHANGE SCHEMA PLAYERS

database/migrations

```
 Schema::create('players', function (Blueprint $table) {
            $table->id();
            $table->string('nim')->unique();
            $table->string('nama');
            $table->enum('kelas', ['MIPA', 'IPS']);
            $table->timestamps();
        });
```

# PROTECTED MODEL PLAYER

models/players

```
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Player extends Model
{
    protected $fillable = ['nim', 'nama', 'kelas'];
}
```

# MIGRATION SCHEMA

```
php artisan migrate
```

# MAKE RESOURCE

```
php artisan make:filament-resource PlayerResource
```

# REQUIRE SCHEMA FOR ADD NEW PLAYERS

app/filament/resources/playerresources.php

```
return $form
            ->schema([
                Card::make()
                    ->schema([
                        TextInput::make('nim')->required(),
                        TextInput::make('nama')->required(),
                        Select::make('kelas')
                            ->options([
                                'MIPA' => 'MIPA',
                                'IPS' =>  'IPS'
                            ]),
                    ])
                    ->columns(2),
            ]);
```

# REQUIRE SCHEMA TABLE FOR SHOW TABLE PLAYERS

at app/filament/resources/playerresources.php.
im add sortable and searchable for get the search input and sorting

```
 public static function table(Table $table): Table
    {
        return $table
            ->columns([
                TextColumn::make('nim')->sortable()->searchable(),
                TextColumn::make('nama')->sortable()->searchable(),
                TextColumn::make('kelas')->sortable()->searchable(),
            ])
            ->filters([
                //
            ])
            ->actions([
                Tables\Actions\EditAction::make(),
            ])
            ->bulkActions([
                Tables\Actions\BulkActionGroup::make([
                    Tables\Actions\DeleteBulkAction::make(),
                ]),
            ]);
    }
```

# EDIT TABLE FOR UNIQUE COLUMN

at app/filament/resources/playerresources.php.
Im add unique ignorable record from nim if duplicate

```
return $form
            ->schema([
                Card::make()
                    ->schema([
                        TextInput::make('nim')->required()->unique(ignorable: fn
                    ($record)=>$record ),
```
