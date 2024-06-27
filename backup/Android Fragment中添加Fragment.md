最好使用 getChildFragmentManager 来获取manager

子fragment调用父fragment的方法：
```java
List<Fragment>list=(List<Fragment>)SonFragment.this.getFragmentManager().getFragments();
for(Fragment f:list){
    if(f!=null&&f instanceof FatherMainFragment){
        ((FatherMainFragment) f).changView();
        break;
    }
}
```

如果是fragment 里的viewpager 中的 fragment  想调用父fragment里的方法  需要通过获取activity来获取FragmentManager
```java
List<Fragment> fragments = getActivity().getSupportFragmentManager().getFragments();
for (Fragment fragment : fragments) {
    if (fragment != null && fragment instanceof WeatherFragment) {
        mHomeFragment = (WeatherFragment) fragment;
    }
}
```


<!-- ##{"timestamp":1546663800}## -->