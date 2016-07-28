# RxStudy

- RxJava
- RxAndroid
- RxBinding
- RxPermissions

## 按钮是否可点击,依赖多个文本不为空
    Observable<CharSequence> rxEmail = RxTextView.textChanges(mEmailView);
    Observable<CharSequence> rxPassword = RxTextView.textChanges(mPasswordView);
    
    Observable.combineLatest(rxEmail, rxPassword, new Func2<CharSequence, CharSequence, Boolean>() {
        @Override
        public Boolean call(CharSequence charSequence, CharSequence charSequence2) {
            //===== 业务逻辑处理按钮什么时候可点击 =====
            if (TextUtils.isEmpty(charSequence) || TextUtils.isEmpty(charSequence2)){
                return false;
            }
            return true;
        }
    }).subscribe(new Observer<Boolean>() {
        @Override
        public void onNext(Boolean o) {
            //===== 设置按钮状态 =====
            mEmailSignInButton.setEnabled(o);
        }
    
        @Override
        public void onCompleted() {
    
        }
    
        @Override
        public void onError(Throwable e) {
    
        }
    });
    
## M系统请求读取权限
    RxPermissions.getInstance(this)
            .request(Manifest.permission.READ_CONTACTS)
            .subscribe(new Action1<Boolean>() {
                @Override
                public void call(Boolean aBoolean) {
                    if (aBoolean) {
                        populateAutoComplete();
                    } else {
                        Toast.makeText(LoginActivity.this, "not grant", Toast.LENGTH_LONG);
                    }
                }
            });