
-github.com-
function OnStart()
{
    alert("called OnStart\nApp Started: " + app.IsStarted());
}
function OnStart()
{
    app.SetMenu( "Start,Stop,Pause" );

    lay = app.CreateLayout( "linear", "" );

    btn = app.CreateButton( "[fa-gear]", -1, -1, "fontawesome" );
    btn.SetOnTouch( app.ShowMenu );
    lay.AddChild( btn );

    app.AddLayout( lay );
}

function OnMenu( item )
{
    app.ShowPopup( item, "Short" );
}
function OnStart()
{
    app.EnableBackKey( false );

    yndExit = app.CreateYesNoDialog("Exit App?");
    yndExit.SetOnTouch( yndExit_OnTouch );

    app.ShowPopup( "Press the back button" );
}

function yndExit_OnTouch(reply)
{
    if(reply == "Yes") app.Exit();
}

function OnBack()
{
    yndExit.Show();
}
//Called when application is paused.
function OnPause()
{
    app.ShowPopup( "OnPause" );
}
//Called when application is resumed.
function OnResume()
{
    app.ShowPopup( "OnResume" );
}
function OnStart()
{
    lay = app.CreateLayout( "linear", "VCenter,FillXY" );

    txt1 = app.CreateText( "" );
    txt1.SetTextSize( 64 );
    lay.AddChild( txt1 );

    txt2 = app.CreateText( "" );
    txt2.SetTextSize( 64 );
    lay.AddChild( txt2 );
    OnConfig();

    app.AddLayout( lay );
}

//Called when screen rotates
function OnConfig()
{
    var orient = app.GetOrientation();
    txt1.SetText(orient);

    if(orient == "Portrait") orient = "Vertical";
    else orient = "Horizontal";

    lay.SetOrientation( orient );
    txt2.SetText( orient );
}
function OnStart()
{
    var now = Date.now();
    app.SetAlarm( "Set", 1234, OnAlarm, Date.now() + 3000 );
    // app.ToBack();
    // app.Exit();
}

//Called when alarm is triggered.
//(Even if your app is closed)
function OnAlarm( id )
{
    app.ShowPopup( "Got Alarm: id = " + id );
}
//Handle data sent from other apps.
function OnData( isStartUp )
{
    //Display intent data.
    var intent = app.GetIntent();
    if( intent )
    {
        //Extract main data.
        var s = "action: " + intent.action + "\n";
        s += "type: " + intent.type + "\n";
        s += "data: " + intent.data + "\n\n";

        //Extract extras.
        s += "extras:\n";
        for( var key in intent.extras )
            s += key+": "+intent.extras[key] + "\n";

        app.Alert( s, "OnData" );
    }
}