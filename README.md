# kr
            $tables = \Illuminate\Support\Facades\DB::select('SHOW TABLES from MashreqDB');
        foreach($tables as $table) {
            $tableName = $table->Tables_in_amex;
            // echo "php artisan krlove:generate:model ".ucfirst($tableName)." --table-name=".$tableName."<br>";
            echo "php artisan krlove:generate:model ".ucfirst($tableName)." --output-path=".__DIR__."/../../Models/Gcbucket/ --namespace=App\\\Models\\\Gcbucket --connection=gcbucket --table-name=".$tableName."<br>";
 
     
        }
        

# laravel-crud-genrator
--Create function IN HomeController.php

            public function gen(){
                $dir=__DIR__.'/../../';
               $scan=   scandir($dir);
               $models=[];

                foreach($scan as $Model){
                    if (preg_match("/[A-Z]+[a-zA-z0-9]+.php/", $Model)){
                        $class = "\App\\".str_replace('.php','', basename($Model));
                        $func = [new $class, 'getFillable'];
                        $fillable=$func(); 
                        $attribute=$rule=$formHtml=$index='';
                        $index="<td>{{\$".strtolower(str_replace('.php','', basename($Model)))."->id}}</td>\n";
                        foreach($fillable as $field){
                            $rule.="'".$field."'=>'required',\n";
                            $attribute.="'".$field."' => '".ucwords(str_replace('_',' ',$field))."',\n";
                            $formHtml.=' <div class="form-group">
                            <?= Form::label(\''.$field.'\', \''.ucwords(str_replace('_',' ',$field)).':\') ?>
                            <?= Form::text(\''.$field.'\', isset($model)?$model->'.$field.':\'\' , [\'class\' => $errors->has(\''.$field.'\') ? \'form-control is-invalid\' : \'form-control\'  ]) ?>
                            @error(\''.$field.'\')<div class="alert alert-danger">{{ $message }}</div>@enderror
                        </div>'."\n\t";
                            $index.="\t\t\t\t\t\t\t<td>{{\$".strtolower(str_replace('.php','', basename($Model)))."->".$field."}}</td>\n";
                        }
                        $models[]=[
                            'ClassName'=>str_replace('.php','', basename($Model)),
                            'file'=>$Model,
                            'Class'=>$class,
                            'FolderName'=>strtolower(str_replace('.php','', basename($Model))),
                            'fillable'=>$fillable,
                            'rule'=>$rule,
                            'attribute'=>$attribute,
                            'Formhtml'=>$formHtml,
                            'Indexhtml'=>$index,
                        ];
                    }
                }
        $html="<?php\n\nnamespace App\Http\Requests;\n\nuse Illuminate\Foundation\Http\FormRequest;
        use !@CLASS@!;\nuse Illuminate\Validation\Rule;\n
        class !@CLASSNAME@!Request extends FormRequest\n{\n    /**\n     * Determine if the user is authorized to make this request.
             *\n     * @return bool\n     */\n    public function authorize()\n    {\n        return true;\n    }\n\n    /**\n     * Get the validation rules that apply to the request.
             *\n     * @return array\n     */\n    public static function rules()\n    {\n        return [
                    !@RULE@!\n        ];\n    }\n\n\n   
        \npublic function messages()\n{\n \t   return [\n        //'first_name.required' => 'A first name is required by message',
                //'email.email'=>'invalid email'\n    ];\n}\n
        public function attributes()\n{\n    return [\n        !@ATTRUBUTE@!\n    ];\n}\n\n  
        }
        ";
        $ControllerHtml="<?php
        //php artisan make:model !@CLASSNAME@! --migration
        /**
         * php artisan migrate
         * php artisan make:controller !@CLASSNAME@!Controller --resource
         * 
         * routes/web.php 
         * Route::resource('!@FOLDERNAME@!', '!@CLASSNAME@!Controller');
         * Route::apiResource('!@FOLDERNAME@!', '!@CLASSNAME@!Controller');
         */
        namespace App\Http\Controllers;

        use Illuminate\Http\Request;
        use Illuminate\Support\Facades\DB;
        use !@CLASS@!;
        use App\Http\Requests\!@CLASSNAME@!Request;


        class !@CLASSNAME@!Controller extends Controller
        {
            /**
             * Display a listing of the resource.
             *
             * @return \Illuminate\Http\Response
             */

            public function __construct()
            {
                \$this->middleware('auth');
            }

            public function index(Request \$request)
            {
            //     \$name = \$request->get('first_name')!=''?\$request->get('first_name'):false;
            //     \$email = \$request->get('email')!=''?\$request->get('email'):false;
            //     \$job_title = \$request->get('job_title')!=''?\$request->get('job_title'):false;
            //     \$city = \$request->get('city')!=''?\$request->get('city'):false;
            //     \$country = \$request->get('country')!=''?\$request->get('country'):false;

            //     //\$gender = \$request->get('gender') != '' ? \$request->get('gender') : -1;
            //    // \$field = \$request->get('field') != '' ? \$request->get('field') : 'name';
            //    // \$sort = \$request->get('sort') != '' ? \$request->get('sort') : 'asc';

                 \$model = new !@CLASSNAME@!();
            //     if(\$name){
            //         \$model=\$model->where('first_name', 'like', '%' . \$name . '%');
            //     }
            //     if(\$email){
            //         \$model=\$model->where('email', 'like', '%' . \$email . '%');
            //     }
            //     if(\$job_title){
            //         \$model=\$model->where('job_title', 'like', '%' . \$job_title . '%');
            //     }
            //     if(\$city){
            //         \$model=\$model->where('city', 'like', '%' . \$city . '%');
            //     }
            //     if(\$country){
            //         \$model=\$model->where('country', 'like', '%' . \$country . '%');
            //     }
                //\$model=\$model->sortable()->paginate(2);
                \$model=\$model->paginate(2);
                return view('!@FOLDERNAME@!.index', compact('model'));
            }

            /**
             * Show the form for creating a new resource.
             *
             * @return \Illuminate\Http\Response
             */
            public function create()
            {
                return view('!@FOLDERNAME@!.create');
            }

            /**
             * Store a newly created resource in storage.
             *
             * @param  \Illuminate\Http\Request  \$request
             * @return \Illuminate\Http\Response
             */

            public function store(!@CLASSNAME@!Request \$request)
            {
                \$validatedData = \$request->validated();

                \$model = new !@CLASSNAME@!;
                //\$data = \$request->only(\$model->getFillable());
                \$data = \$request->all();
                \$model->fill(\$data)->save();
                return redirect('/!@FOLDERNAME@!')->with('success', '!@CLASSNAME@! saved!');
            }



            /**
             * Display the specified resource.
             *
             * @param  int  \$id
             * @return \Illuminate\Http\Response
             */
            public function show(int \$id)
            {
                //
            }

            /**
             * Show the form for editing the specified resource.
             *
             * @param  int  \$id
             * @return \Illuminate\Http\Response
             */
            public function edit(int \$id)
            {
                \$model = !@CLASSNAME@!::find(\$id);
                return view('!@FOLDERNAME@!.edit', compact('model'));    
            }

            /**
             * Update the specified resource in storage.
             *
             * @param  \Illuminate\Http\Request  \$request
             * @param  int  \$id
             * @return \Illuminate\Http\Response
             */
            public function update(!@CLASSNAME@!Request \$request,int \$id)
            {
                \$request->validated();
                \$model = !@CLASSNAME@!::find(\$id);
                //\$data = \$request->only(\$model->getFillable());
                 \$data = \$request->all();
                 \$model->fill(\$data)->save();
                return redirect('/!@FOLDERNAME@!')->with('success', '!@CLASSNAME@! updated!');
            }

            /**
             * Remove the specified resource from storage.
             *
             * @param  int  \$id
             * @return \Illuminate\Http\Response
             */
            public function destroy(int \$id)
            {
                \$model = !@CLASSNAME@!::find(\$id);
                \$model->delete();

                return redirect('/!@FOLDERNAME@!')->with('success', '!@CLASSNAME@! deleted!');
            }
        }
        ";
        //echo "71<pre>";print_r($models[0]);exit;

                foreach($models as $MODEL){
                    $htmlCopy=$html;
                    $ControllerHtmlCopy=$ControllerHtml;
                    $replce=[
                        '!@CLASS@!'=>$MODEL['Class'],
                        '!@CLASSNAME@!'=>str_replace('.php','', basename($MODEL['file'])),
                        '!@RULE@!'=>$MODEL['rule'],
                        '!@ATTRUBUTE@!'=>$MODEL['attribute'],
                        '!@FOLDERNAME@!'=>$MODEL['FolderName'],

                    ];
                    foreach($replce as $k=>$v){
                        $htmlCopy=str_replace($k,$v,$htmlCopy);
                        $ControllerHtmlCopy=str_replace($k,$v,$ControllerHtmlCopy);
                    }
                    $fileName=str_replace('.php','', basename($MODEL['file'])).'Request.php';
                    $ControllerfileName=str_replace('.php','', basename($MODEL['file'])).'Controller.php';
                    file_put_contents($dir.'/Http/Requests/'.$fileName,$htmlCopy);
                    file_put_contents($dir.'/Http/Controllers/'.$ControllerfileName,$ControllerHtmlCopy);
                }

            //    echo "<pre>";

            //   print_r($models[0]);exit;
               $dir= $dir.'/../resources/views/';
               $Folder='testdir';
               $index='index.blade';
               $create='create.blade';
               $edit='edit.blade';
               $form='form.blade';

               $formHTML="<?php 
               /**
                * !@CONTROLLER@! Form
                */
               ?>
               @if(isset(\$model))
                   {{ Form::model(\$model, ['route' => ['!@CONTROLLER@!.update', \$model->id], 'method' => 'patch']) }}
               @else
                   {{ Form::open(['route' => '!@CONTROLLER@!.store']) }}
               @endif

               ~!@FIELDS@!~


                         <button type=\"submit\" class=\"btn btn-primary-outline\">{{isset(\$model)?'Update':'Add'}} !@CONTROLLER@!</button>
                       <?= Form::close() ?>
               ";
               $createHTML="@extends('layouts.app')

               @section('content')
               <div class=\"row\">
                <div class=\"col-sm-8 offset-sm-2\">
                   <h1 class=\"display-3\">{{isset(\$model)?'Edit':'Add'}} a !@FOLDER@!</h1>
                 <div>

                   @include('!@FOLDER@!.form')
                 </div>
               </div>
               </div>
               @endsection";

               $editHTML="@extends('layouts.app')

               @section('content')
               <div class=\"row\">
                <div class=\"col-sm-8 offset-sm-2\">
                   <h1 class=\"display-3\"> {{isset(\$model)?'Edit':'Add'}} a !@FOLDER@!</h1>
                 <div>

                   @include('!@FOLDER@!.form')
                 </div>
               </div>
               </div>
               @endsection";

               $indexHTML="<?php

               Form::macro('SearchField', function(\$name)
               {
                 //echo \"<pre>\";print_r(\$_GET);exit;
                 \$var=isset(\$_GET[\$name])?\$_GET[\$name]:\"\";
                 \$return ='
                 <form method=\"get\">
                 <div class=\"input-group\">';
                 if(isset(\$_GET)){
                   foreach(\$_GET as \$k=>\$v){
                     \$return .='<input type=\"hidden\" name=\"'.\$k.'\" value=\"'.\$v.'\">';
                   }
                 }
                 \$return .='<input type=\"text\" value=\"'.\$var.'\" class=\"form-control\" name=\"'.\$name.'\" placeholder=\"Search '.\$name.' \"> 
                   </div> </form>';
                 return \$return;
               });

               ?>

               @extends('layouts.app')

               @section('content')
               <div class=\"row\">
               <div class=\"col-sm-12\">
               @if(session()->get('success'))
                   <div class=\"alert alert-success\">
                     {{ session()->get('success') }}  
                   </div>
               @endif

               <div class=\"container\">

               </div>

                   <h1 class=\"display-3\">!@FOLDER@!</h1>  
                               <a href=\"{{ route('!@FOLDER@!.create')}}\" class=\"btn btn-primary\">Create New Record</a>

                 <table class=\"table table-striped\">


                   <tbody>
                       @foreach(\$model as \$!@FOLDER@!)
                       <tr>
                       !@INDEXHTML@!
                           <td>
                               <a href=\"{{ route('!@FOLDER@!.edit',\$!@FOLDER@!->id)}}\" class=\"btn btn-primary\">Edit</a>
                           </td>
                           <td>
                               <form action=\"{{ route('!@FOLDER@!.destroy', \$!@FOLDER@!->id)}}\" method=\"post\">
                                 @csrf
                                 @method('DELETE')
                                 <button class=\"btn btn-danger\" onclick=\"return confirm('Are you sure?')\" type=\"submit\">Delete</button>
                               </form>
                           </td>
                       </tr>
                       @endforeach

                   </tbody>
                 </table>
                 {{ \$model->links() }} 

               <div>
               </div>

               @endsection

               ";


               //mkdir($dir.'/'.strtolower($MODEL['ClassName']));
               foreach($models as $MODEL){

                $filePath=$dir.'/'.strtolower($MODEL['ClassName']);
                if (!file_exists($filePath)) {
                    mkdir($filePath, 0777, true);
                }
                $htmlCopy=$formHTML;
                $replce=[
                    '~!@FIELDS@!~'=>$MODEL['Formhtml'],
                    '!@CONTROLLER@!'=>strtolower($MODEL['ClassName']),


                ];
                foreach($replce as $k=>$v){
                    $htmlCopy=str_replace($k,$v,$htmlCopy);
                }
                file_put_contents($filePath.'/form.blade.php',$htmlCopy); 
                /****
                 * CREATE PAGE
                 */
                $htmlCopyCreate=$createHTML;
                $htmlCopyEdit=$editHTML;
                $htmlCopyIndex=$indexHTML;
                $replce=[
                    '!@FOLDER@!'=>strtolower($MODEL['ClassName']),
                    '!@INDEXHTML@!'=>$MODEL['Indexhtml'],
                ];
                foreach($replce as $k=>$v){
                    $htmlCopyCreate=str_replace($k,$v,$htmlCopyCreate);
                    $htmlCopyEdit=str_replace($k,$v,$htmlCopyEdit);
                    $htmlCopyIndex=str_replace($k,$v,$htmlCopyIndex);
                }
                file_put_contents($filePath.'/create.blade.php',$htmlCopyCreate); 
                file_put_contents($filePath.'/edit.blade.php',$htmlCopyEdit); 
                file_put_contents($filePath.'/index.blade.php',$htmlCopyIndex); 
                //file_put_contents($dir.'/Http/Requests/'.$fileName,$htmlCopy);
                }

            //    file_put_contents($filePath.'/form.blade',$htmlCopy);
            //    $scan=   scandir($dir);
            //    print_r($scan);
            echo "success";
               exit;

               print_r($models);
            }
 
        
#composer.json

        "laravelcollective/html": "~5.0"
        
#config/app.php

        aliases => 

           'Form' => 'Collective\Html\FormFacade',
            'Html' => 'Collective\Html\HtmlFacade',

        providers => 
           'Collective\Html\HtmlServiceProvider',
           
#routes/web.php

        Route::get('/gen', 'HomeController@gen')->name('gen');
        
#CREATE FOLDER app/http/Requests

