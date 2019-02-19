Installing magento 2.3
=====================================
1. use incognito browser to avoid {{menuState.url}} error
2. After installation, a brown blank page will probably display on the admin page --
run on terminal or cmd (php .\bin\magento setup:static-content:deploy -f).
3.  Open magento\lib\internal\Magento\Framework\View\Element\Template\File\Validator.php then replace public function isValid($filename)
{
    $filename = str_replace('\\', '/', $filename);
    if (!isset($this->_templatesValidationResults[$filename])) {
        $this->_templatesValidationResults[$filename] =
            ($this->isPathInDirectories($filename, $this->_compiledDir)
                || $this->isPathInDirectories($filename, $this->moduleDirs)
                || $this->isPathInDirectories($filename, $this->_themesDir)
                || $this->_isAllowSymlinks)
            && $this->getRootDirectory()->isFile($this->getRootDirectory()->getRelativePath($filename));
    }
    return $this->_templatesValidationResults[$filename];
}
with
 
public function isValid($filename)
    {
       return true;
    }
4. Installing sample data procedure
    a. composer config repositories.magento composer https://repo.magento.com/packages.json
    b. composer update ( Need to input your mage market username and password)
    c. magento sampledata:deploy
    d. magento setup:upgrade
5. Magento2 admin menu panel doesn't work
    a. Got to app/etc/di.xml and find the line 
    Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink
    and replace with Magento\Framework\App\View\Asset\MaterializationStrategy\Copy
6. Magento Logo not loading
    a. Open cmd or terminal and run php bin/magento setup:static-content:deploy in the magento directory.
7. Cannot replace Luma logo, unable to upload logo
    a. Got to app/code/Magento/Theme/view/adminhtml/ui_component/design_config_form.xml
    b. Relace <field name="head_shortcut_icon" formElement="fileUploader"> with <field name="head_shortcut_icon" formElement="imageUploader">