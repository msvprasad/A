package com.saharsh.chatapp;
import android.content.Intent;
import android.graphics.Typeface;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View; 
import android.widget.Button;
import android.widget.TextView;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;
public class StartActivity extends AppCompatActivity { 
Button login, register;
TextView chat_title_tv;
Typeface MR, MRR;
FirebaseUser firebaseUser;
@Override
protected void onStart() { 
super.onStart();
firebaseUser = FirebaseAuth.getInstance().getCurrentUser();
//check if user is null
if (firebaseUser != null){
Intent intent = new Intent(StartActivity.this, MainActivity.class);
startActivity(intent);
finish();
}
}
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_start);
MRR = Typeface.createFromAsset(getAssets(), "fonts/myriadregular.ttf");
MR = Typeface.createFromAsset(getAssets(), "fonts/myriad.ttf");
login = findViewById(R.id.login);
register = findViewById(R.id.register);
chat_title_tv = findViewById(R.id.chat_title_tv);
login.setTypeface(MR);
register.setTypeface(MR);
chat_title_tv.setTypeface(MR);
login.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
startActivity(new Intent(StartActivity.this, LoginActivity.class));
}
});
register.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
startActivity(new Intent(StartActivity.this, RegisterActivity.class));
}
});
}
}
package com.saharsh.chatapp;
import android.app.ProgressDialog;
import android.content.Intent;
import android.graphics.Typeface;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;
public class LoginActivity extends AppCompatActivity {
EditText email, password;
Button btn_login;
Typeface MR,MRR;
ProgressDialog dialog;
FirebaseAuth auth;
TextView forgot_password, login_tv, msg_tv;
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_login);
MRR = Typeface.createFromAsset(getAssets(), "fonts/myriadregular.ttf");
MR = Typeface.createFromAsset(getAssets(), "fonts/myriad.ttf");
Toolbar toolbar = findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
getSupportActionBar().setTitle("Login");
getSupportActionBar().setDisplayHomeAsUpEnabled(true);
auth = FirebaseAuth.getInstance();
login_tv = findViewById(R.id.login_tv);
email = findViewById(R.id.email)
 password = findViewById(R.id.password);
btn_login = findViewById(R.id.btn_login);
forgot_password = findViewById(R.id.forgot_password);
msg_tv = findViewById(R.id.msg_tv); 
msg_tv.setTypeface(MRR);
login_tv.setTypeface(MR);
email.setTypeface(MRR);
password.setTypeface(MRR);
btn_login.setTypeface(MRR);
forgot_password.setTypeface(MRR);
forgot_password.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
startActivity(new Intent(LoginActivity.this, ResetPasswordActivity.class));
}
});
btn_login.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
String txt_email = email.getText().toString();
String txt_password = password.getText().toString();
Utils.hideKeyboard(LoginActivity.this);
if (TextUtils.isEmpty(txt_email) || TextUtils.isEmpty(txt_password)){ Toast.makeText(LoginActivity.this,	"All	fields	are	required",
Toast.LENGTH_SHORT).show();
} else {
dialog = Utils.showLoader(LoginActivity.this);
auth.signInWithEmailAndPassword(txt_email, txt_password)
.addOnCompleteListener(new OnCompleteListener<AuthResult>() {
@Override
public void onComplete(@NonNull Task<AuthResult> task) {
if (task.isSuccessful()){ 
Intent	intent	=	new	Intent(LoginActivity.this,MainActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK	|Intent.FLAG_ACTIVITY_NEW_TASK);
if(dialog!=null){
dialog.dismiss();
}
startActivity(intent);
finish();
} else {
if(dialog!=null){
dialog.dismiss();
}
Toast.makeText(LoginActivity.this,	"Authentication	failed!", Toast.LENGTH_SHORT).show();
}
}
});
}
}
});
}
}
package com.saharsh.chatapp;

import android.app.ProgressDialog;
import android.content.Intent;
import android.graphics.Typeface;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.gms.tasks.OnCompleteListener;
 
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;
import com.rengwuxian.materialedittext.MaterialEditText;

import java.util.HashMap;
public class RegisterActivity extends AppCompatActivity {
EditText username, email, password;
TextView register_tv, msg_reg_tv;
Button btn_register;
Typeface MR, MRR;
FirebaseAuth auth;
DatabaseReference reference;
ProgressDialog dialog;

@Override
protected void onCreate(Bundle savedInstanceState) { 
super.onCreate(savedInstanceState); 
setContentView(R.layout.activity_register);

MRR = Typeface.createFromAsset(getAssets(), "fonts/myriadregular.ttf"); 
MR = Typeface.createFromAsset(getAssets(), "fonts/myriad.ttf");

Toolbar toolbar = findViewById(R.id.toolbar);
setSupportActionBar(toolbar); 
getSupportActionBar().setTitle("Register"); 
getSupportActionBar().setDisplayHomeAsUpEnabled(true);

username = findViewById(R.id.username); 
email = findViewById(R.id.email); 
password = findViewById(R.id.password); 
btn_register = findViewById(R.id.btn_register); 
register_tv = findViewById(R.id.register_tv); 
msg_reg_tv = findViewById(R.id.msg_reg_tv);

msg_reg_tv.setTypeface(MRR);
username.setTypeface(MRR); 
email.setTypeface(MRR); 
password.setTypeface(MRR); 
btn_register.setTypeface(MR); 
register_tv.setTypeface(MR);
auth = FirebaseAuth.getInstance(); 
btn_register.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
String txt_username = username.getText().toString(); 
String txt_email = email.getText().toString();
String txt_password = password.getText().toString(); 
Utils.hideKeyboard(RegisterActivity.this);

if(TextUtils.isEmpty(txt_username)	||	TextUtils.isEmpty(txt_email)	|| TextUtils.isEmpty(txt_password)){
Toast.makeText(RegisterActivity.this,	"All	fileds	are	required", Toast.LENGTH_SHORT).show();
} else if (txt_password.length() < 6 ){
Toast.makeText(RegisterActivity.this, "password must be at least 6 characters", Toast.LENGTH_SHORT).show();
} else {
register(txt_username, txt_email, txt_password);
}
}
});
}
 

private void register(final String username, String email, String password){ dialog = Utils.showLoader(RegisterActivity.this);
auth.createUserWithEmailAndPassword(email, password)
.addOnCompleteListener(new OnCompleteListener<AuthResult>() { @Override
public void onComplete(@NonNull Task<AuthResult> task) { if (task.isSuccessful()){
FirebaseUser firebaseUser = auth.getCurrentUser(); assert firebaseUser != null;
String userid = firebaseUser.getUid();

reference	=
FirebaseDatabase.getInstance().getReference("Users").child(userid);

HashMap<String, String> hashMap = new HashMap<>(); hashMap.put("id", userid);
hashMap.put("username", username); hashMap.put("imageURL", "default"); hashMap.put("status", "offline"); hashMap.put("bio", "");
hashMap.put("search", username.toLowerCase()); if(dialog!=null){
dialog.dismiss();
}
reference.setValue(hashMap).addOnCompleteListener(new OnCompleteListener<Void>() {
@Override
public void onComplete(@NonNull Task<Void> task) { if (task.isSuccessful()){
Intent	intent	=	new	Intent(RegisterActivity.this,
MainActivity.class);
 
intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK	| Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent); finish();
}
}
});
} else {
Toast.makeText(RegisterActivity.this, "You can't register woth this email or password", Toast.LENGTH_SHORT).show();
if(dialog!=null){ dialog.dismiss();
}
}
}
});
}
}
package com.saharsh.chatapp;

import android.content.Context;
import android.support.annotation.NonNull; import android.support.annotation.Nullable;
import android.support.design.widget.BottomSheetDialogFragment; import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.LayoutInflater; import android.view.View;
import android.view.ViewGroup; import android.widget.ImageView; import android.widget.TextView;

import com.bumptech.glide.Glide;
import com.google.firebase.database.DataSnapshot; import com.google.firebase.database.DatabaseError;
 
import com.google.firebase.database.DatabaseReference; import com.google.firebase.database.FirebaseDatabase; import com.google.firebase.database.ValueEventListener; import com.saharsh.chatapp.Model.User;
public class ViewProfileActivity extends BottomSheetDialogFragment { String uid;
DatabaseReference reference;
TextView username, bio_et; ImageView profile_img; static Context mContext;


public ViewProfileActivity() {

}

public static ViewProfileActivity newInstance(String uid, Context context) {

Bundle args = new Bundle(); args.putString("uid",uid); mContext = context;
ViewProfileActivity fragment = new ViewProfileActivity(); fragment.setArguments(args);
return fragment;
}

@Override
public void onCreate(@Nullable Bundle savedInstanceState) { super.onCreate(savedInstanceState);


}
 
@Nullable @Override
public	View	onCreateView(@NonNull	LayoutInflater	inflater,	@Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
View view = inflater.inflate(R.layout.activity_view_profile,container,false); if(getArguments()!=null){
uid = getArguments().getString("uid");
profile_img = view.findViewById(R.id.view_profile_image); username = view.findViewById(R.id.view_username); bio_et = view.findViewById(R.id.view_bio_et);

reference = FirebaseDatabase.getInstance().getReference("Users").child(uid); reference.addValueEventListener(new ValueEventListener() {
@Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { User user = dataSnapshot.getValue(User.class); username.setText(user.getUsername()); bio_et.setText(user.getBio());
if (user.getImageURL().equals("default")){ profile_img.setImageResource(R.drawable.profile_img);
} else {
//change this

Glide.with(mContext.getApplicationContext()).load(user.getImageURL()).into(profile
_img);
}
}

@Override
public void onCancelled(@NonNull DatabaseError databaseError) {

}
});
}
 
return view;
}
}

package com.saharsh.chatapp;

import android.app.Activity;
import android.app.ProgressDialog; import android.content.Context;
import android.graphics.drawable.ColorDrawable; import android.view.View;
import android.view.WindowManager;
import android.view.inputmethod.InputMethodManager; import android.widget.ProgressBar;

public class Utils {
 

public static ProgressDialog showLoader(Context context){ ProgressDialog dialog = new ProgressDialog(context); try {
dialog.show();
} catch (WindowManager.BadTokenException e) {

}
dialog.setCancelable(false); dialog.getWindow()
.setBackgroundDrawable(new ColorDrawable(android.graphics.Color.TRANSPARENT));
dialog.setContentView(R.layout.progressdialog); return dialog;
// dialog.setMessage(Message);
}

public static void hideKeyboard(Activity activity) {
InputMethodManager	imm	=	(InputMethodManager) activity.getSystemService(Activity.INPUT_METHOD_SERVICE);
//Find the currently focused view, so we can grab the correct window token from
 
it.
 

View view = activity.getCurrentFocus();
//If no view currently has focus, create a new one, just so we can grab a window
 
token from it
if (view == null) {
view = new View(activity);
}
imm.hideSoftInputFromWindow(view.getWindowToken(), 0);
}

public static void hideLoader(ProgressDialog dialog){
// To dismiss the dialog if(dialog!=null){ dialog.dismiss();
 
}
}
}


package com.saharsh.chatapp;

import android.content.Intent;
import android.content.SharedPreferences; import android.graphics.Typeface;
import android.os.Bundle;
import android.support.annotation.NonNull; import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.LinearLayoutManager; import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar; import android.view.View;
import android.widget.EditText;
 
import android.widget.ImageButton; import android.widget.TextView; import android.widget.Toast;

import com.bumptech.glide.Glide;
import com.google.firebase.auth.FirebaseAuth; import com.google.firebase.auth.FirebaseUser; import com.google.firebase.database.DataSnapshot; import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference; import com.google.firebase.database.FirebaseDatabase; import com.google.firebase.database.Query;
import com.google.firebase.database.ValueEventListener; import com.saharsh.chatapp.Adapter.MessageAdapter; import com.saharsh.chatapp.Fragments.APIService; import com.saharsh.chatapp.Model.Chat;
import com.saharsh.chatapp.Model.User;
import com.saharsh.chatapp.Notifications.Client; import com.saharsh.chatapp.Notifications.Data;
import com.saharsh.chatapp.Notifications.MyResponse; import com.saharsh.chatapp.Notifications.Token; import com.saharsh.chatapp.Notifications.Sender;

import java.util.ArrayList; import java.util.HashMap; import java.util.List;

import de.hdodenhof.circleimageview.CircleImageView; import retrofit2.Call;
import retrofit2.Callback; import retrofit2.Response;

public class MessageActivity extends AppCompatActivity { CircleImageView profile_image;
 
TextView username;

Typeface MR, MRR;

FirebaseUser fuser; DatabaseReference reference;

ImageButton btn_send; EditText text_send;

MessageAdapter messageAdapter; List<Chat> mchat;

RecyclerView recyclerView; Intent intent;
ValueEventListener seenListener; String userid;
APIService apiService; boolean notify = false;
@Override
protected void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); setContentView(R.layout.activity_message);

MRR = Typeface.createFromAsset(getAssets(), "fonts/myriadregular.ttf"); MR = Typeface.createFromAsset(getAssets(), "fonts/myriad.ttf");

Toolbar toolbar = findViewById(R.id.toolbar); setSupportActionBar(toolbar);
 
getSupportActionBar().setTitle(""); getSupportActionBar().setDisplayHomeAsUpEnabled(true); toolbar.setNavigationOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View view) {
// and this
startActivity(new	Intent(MessageActivity.this, MainActivity.class).setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP));
}
});

apiService	=
Client.getClient("https://fcm.googleapis.com/").create(APIService.class);

recyclerView = findViewById(R.id.recycler_view); recyclerView.setHasFixedSize(true);
LinearLayoutManager	linearLayoutManager	=	new LinearLayoutManager(getApplicationContext());
linearLayoutManager.setStackFromEnd(true); recyclerView.setLayoutManager(linearLayoutManager);

profile_image = findViewById(R.id.profile_image); username = findViewById(R.id.username); btn_send = findViewById(R.id.btn_send); text_send = findViewById(R.id.text_send);

username.setTypeface(MR); text_send.setTypeface(MRR);


intent = getIntent();
userid = intent.getStringExtra("userid");
fuser = FirebaseAuth.getInstance().getCurrentUser(); btn_send.setOnClickListener(new View.OnClickListener() {
 
@Override
public void onClick(View view) { notify = true;
String msg = text_send.getText().toString();
String time = String.valueOf(System.currentTimeMillis()); if (!msg.equals("")){
sendMessage(fuser.getUid(), userid, msg, time);
} else {
Toast.makeText(MessageActivity.this, "You can't send empty message", Toast.LENGTH_SHORT).show();
}
text_send.setText("");
}
});



reference	=
FirebaseDatabase.getInstance().getReference("Users").child(userid);

reference.addValueEventListener(new ValueEventListener() { @Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { User user = dataSnapshot.getValue(User.class); username.setText(user.getUsername());
if (user.getImageURL().equals("default")){ profile_image.setImageResource(R.drawable.profile_img);
} else {
//and this

Glide.with(getApplicationContext()).load(user.getImageURL()).into(profile_image);
}

readMesagges(fuser.getUid(), userid, user.getImageURL());
}
 
@Override
public void onCancelled(@NonNull DatabaseError databaseError) {

}
});

seenMessage(userid);
}

private void seenMessage(final String userid){
reference = FirebaseDatabase.getInstance().getReference("Chats"); seenListener = reference.addValueEventListener(new ValueEventListener() {
@Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { for (DataSnapshot snapshot : dataSnapshot.getChildren()){
Chat chat = snapshot.getValue(Chat.class);
if	(chat.getReceiver().equals(fuser.getUid())	&& chat.getSender().equals(userid)){
HashMap<String, Object> hashMap = new HashMap<>(); hashMap.put("isseen", true); snapshot.getRef().updateChildren(hashMap);
}
}
}

@Override
public void onCancelled(@NonNull DatabaseError databaseError) {

}
});
}

private void sendMessage(String sender, final String receiver, String message, String time){
 
DatabaseReference reference = FirebaseDatabase.getInstance().getReference();

HashMap<String, Object> hashMap = new HashMap<>(); hashMap.put("sender", sender); hashMap.put("receiver", receiver); hashMap.put("message", message); hashMap.put("isseen", false);
hashMap.put("time", time); reference.child("Chats").push().setValue(hashMap);
// add user to chat fragment
final	DatabaseReference	chatRef	= FirebaseDatabase.getInstance().getReference("Chatlist")
.child(fuser.getUid())
.child(userid);
chatRef.addListenerForSingleValueEvent(new ValueEventListener() { @Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { if (!dataSnapshot.exists()){
chatRef.child("id").setValue(userid);
}
}
@Override
public void onCancelled(@NonNull DatabaseError databaseError) {
}
});
final	DatabaseReference	chatRefReceiver	= FirebaseDatabase.getInstance().getReference("Chatlist")
.child(userid)
.child(fuser.getUid()); chatRefReceiver.child("id").setValue(fuser.getUid());
final String msg = message
reference	=
FirebaseDatabase.getInstance().getReference("Users").child(fuser.getUid()); reference.addValueEventListener(new ValueEventListener() {
@Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { User user = dataSnapshot.getValue(User.class);
if (notify) {
sendNotifiaction(receiver, user.getUsername(), msg);
}
notify = false;
}
@Override
public void onCancelled(@NonNull DatabaseError databaseError) {
}
});
}
private void sendNotifiaction(String receiver, final String username, final String message){
DatabaseReference	tokens	= FirebaseDatabase.getInstance().getReference("Tokens");
Query query = tokens.orderByKey().equalTo(receiver); query.addValueEventListener(new ValueEventListener() {
@Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { for (DataSnapshot snapshot : dataSnapshot.getChildren()){
Token token = snapshot.getValue(Token.class); 
Data	data	=	new	Data(fuser.getUid(),	R.drawable.profile_img, username+": "+message, "New Message",
userid);
Sender sender = new Sender(data, token.getToken()); apiService.sendNotification(sender)
.enqueue(new Callback<MyResponse>() { @Override
public	void	onResponse(Call<MyResponse>	call, Response<MyResponse> response) {
if (response.code() == 200){
if (response.body().success != 1){
//Toast.makeText(MessageActivity.this,	"Failed!", Toast.LENGTH_SHORT).show();
}
}
}
@Override
public void onFailure(Call<MyResponse> call, Throwable t) {
}
});
}
}
@Override
public void onCancelled(@NonNull DatabaseError databaseError) {
}
});
}
private void readMesagges(final String myid, final String userid, final String imageurl){
mchat = new ArrayList<>();
reference = FirebaseDatabase.getInstance().getReference("Chats"); reference.addValueEventListener(new ValueEventListener() {
@Override
public void onDataChange(@NonNull DataSnapshot dataSnapshot) { mchat.clear();
for (DataSnapshot snapshot : dataSnapshot.getChildren()){ Chat chat = snapshot.getValue(Chat.class);
if (chat.getReceiver().equals(myid) && chat.getSender().equals(userid) || chat.getReceiver().equals(userid)	&&
chat.getSender().equals(myid)){
mchat.add(chat);
}
imageurl);
}
}
messageAdapter = new MessageAdapter(MessageActivity.this, mchat, recyclerView.setAdapter(messageAdapter);
@Override
public void onCancelled(@NonNull DatabaseError databaseError) {
}
});
}
private void currentUser(String userid){
SharedPreferences.Editor	editor	=	getSharedPreferences("PREFS", MODE_PRIVATE).edit();
editor.putString("currentuser", userid); editor.apply(); 
}
private void status(String status){
reference	=
FirebaseDatabase.getInstance().getReference("Users").child(fuser.getUid());
HashMap<String, Object> hashMap = new HashMap<>(); hashMap.put("status", status);
reference.updateChildren(hashMap);
}
@Override
protected void onResume() { super.onResume(); status("online"); currentUser(userid);
}
@Override
protected void onPause() { super.onPause();
reference.removeEventListener(seenListener); status("offline");
currentUser("none");
}
}
 
