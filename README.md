# ColorAnimation and Flyout issue repo
Steps to replicate:

1. Create a `Button` on the page and attach a `Flyout` to it. Then create a `TextBox` within the `Flyout`.
2. Use Blend to generate a default `Style` for the `TextBox`.
3. Add the following `VisualTransition` to the `CommonStates` visual state group. Note the `GeneratedDuration` can be anything that's greater than `0`.
 
 ```xml
    <VisualStateGroup.Transitions>
        <VisualTransition GeneratedDuration="0:0:0.4" />
    </VisualStateGroup.Transitions>
 ```
 
4. Modify the `PointerOver` visual state by adding a new `ColorAnimation` for `BorderElement.BorderBrush` and commenting out the ones that involve `BorderElement`.
 
 ```xml
   <VisualState x:Name="PointerOver">
      <Storyboard>
          <!--  Add Color animation to BorderElement  -->
          <ColorAnimation Duration="0"
                          Storyboard.TargetName="BorderElement"
                          Storyboard.TargetProperty="(Border.BorderBrush).(SolidColorBrush.Color)"
                          To="LightYellow"
                          d:IsOptimized="True" />

          <!--  Comment out other built-in animations on BorderElement  -->
          <!--<ObjectAnimationUsingKeyFrames Storyboard.TargetName="BorderElement" Storyboard.TargetProperty="BorderBrush">
              <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource TextControlBorderBrushPointerOver}" />
          </ObjectAnimationUsingKeyFrames>
          <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BorderElement" Storyboard.TargetProperty="Background">
              <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource TextControlBackgroundPointerOver}" />
          </ObjectAnimationUsingKeyFrames>-->
          <ObjectAnimationUsingKeyFrames Storyboard.TargetName="PlaceholderTextContentPresenter" Storyboard.TargetProperty="Foreground">
              <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource TextControlPlaceholderForegroundPointerOver}" />
          </ObjectAnimationUsingKeyFrames>
          <ObjectAnimationUsingKeyFrames Storyboard.TargetName="ContentElement" Storyboard.TargetProperty="Foreground">
              <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource TextControlForegroundPointerOver}" />
          </ObjectAnimationUsingKeyFrames>
      </Storyboard>
  </VisualState>
 ```
 
5. Now, you need *three clicks* to cause an app crash. First *click* is on the `Button` to bring up the `Flyout`. Then you perform a second *click* anywhere on the `Flyout`. After that, make sure your mouse cursor goes past the `TextBox` to trigger the `PointerOver` state, and then quickly *click* outside of the `Flyout` to dismiss it. If you do them quickly, you will see the app crash without throwing anything.

Please also note that this doesn't just happen to `TextBox`. I have encountered similar issues with `ComboBox`, `AutoSuggestBox`, `ToggleSwitch` and `Button`. So looks like it's a common issue to me.
    
